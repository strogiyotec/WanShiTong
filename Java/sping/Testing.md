## Spring boot Test
> A nice feature of the Spring Test support is that the application context is cached between tests.
> That way, if you have multiple methods in a test case or multiple test cases with the same configuration,
> they incur the cost of starting the application only once.
> You can control the cache by using the @DirtiesContext annotation.

## Spring boot jdbc test
In order to test jdbc layer with configured flyway and test container you should use this code snipped
```
@ExtendWith(SpringExtension.class)//for junit5
@JdbcTest //jdbcTemplate
@ContextConfiguration(
				classes = UsersRepositoryTestCase.UserRepositoryConfig.class,
				initializers = DatabaseIntegrationTest.Initializer.class
)
@AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
class UsersRepositoryTestCase extends DatabaseIntegrationTest {

    @Autowired
    private UsersRepository repository;

    @Test
    @Sql("/create_test_user.sql")
    @DisplayName("Execute sql script and checks if user with email exists")
    void testGetEmail() {
    }

    @TestConfiguration
    public static class UserRepositoryConfig {

        @Bean
        public JdbcTemplate jdbcTemplate(@Autowired DataSource dataSource) {
            return new JdbcTemplate(dataSource);
        }

        @Bean
        public UsersRepository repository() {
            return new UsersRepository();
        }
    }

}
```

And here is Test container config class
```
abstract class DatabaseIntegrationTest {

    static final PostgreSQLContainer<?> container = new PostgreSQLContainer<>("postgres:11.3")
        .withDatabaseName("jwitter")
        .withUsername("testuser")
        .withPassword("pass")
        .withReuse(true);

    static {
        DatabaseIntegrationTest.container.start();
    }

    public static class Initializer
        implements ApplicationContextInitializer<ConfigurableApplicationContext> {
        public void initialize(final ConfigurableApplicationContext configurableApplicationContext) {
            TestPropertyValues.of(
                "spring.datasource.url=" + DatabaseIntegrationTest.container.getJdbcUrl(),
                "spring.datasource.username=" + DatabaseIntegrationTest.container.getUsername(),
                "spring.datasource.password=" + DatabaseIntegrationTest.container.getPassword()
            ).applyTo(configurableApplicationContext.getEnvironment());
        }
    }
}
```

## Spring Boot Controller Test
```
//Config controllers
@WebMvcTest(controllers = ProjectRestService.class)
//If you wanna import additional config
@Import({WebConfig.class, SecurityJwtConfig.class})
//To use BeforeAll on non static methods
@TestInstance(TestInstance.Lifecycle.PER_CLASS)
class ProjectRestServiceTest {

    @Autowired
    private MockMvc mockMvc;

    //Service mocks for controller
    @MockBean
    private UserService userService;

    @Test
	//If endpoint is secured then mock a user
    @WithMockUser
    @DisplayName("Test that coordinates filed is present in response")
    void testProjectTable() throws Exception {
        this.mockMvc.perform(MockMvcRequestBuilders.get("/svc/projects/table"))
            .andDo(print())
            .andExpect(status().isOk())
            .andExpect(jsonPath("$[0].coordinates").exists());
    }

}

```
## Junit BeforeAll
IF you controller has some services we do inject them using @MockBean
annotation, however if we want to use them in @BeforeAll then it's not 
possible because @BeforeAll method has to be statis.
**Solution** - annotate class with `@TestInstance(TestInstance.Lifecycle.PER_CLASS)`

> Before all of parent class is executed before all @BeforeAll of child classes

## Mockito

### Inject mocks for service

Let's say service has two other services inside. How to test it ? 
```
@ExtendWith(SpringExtension.class)
class ProjectServiceTest {

	//this one is a service we wanna test
    @InjectMocks
    private final ProjectService projectService = new ProjectServiceImpl();
    //here we have a list of services used by projectService
    @Mock
    private UserService userService;
    @Mock
    private AccessControlService accessControlService;
```

### Multiple InjectLevels
Now let's say **projectService** has a **securityService** as a dependency which has **AccessControlService** as a dependency<br>
How to inject **AccessControlService** to security and then securityService to projectService? Use **spy**

```
@ExtendWith(SpringExtension.class)
class ProjectServiceTest {

    @Mock
    private AccessControlService accessControlService;
    @InjectMocks
    private ProjectSecurityService projectSecurityService = spy(ProjectSecurityServiceImpl.class);
    @InjectMocks
    private final ProjectService projectService = new ProjectServiceImpl();
```

### Mockito reset
For every new test , Mockito creates new Mocks. The main reason is that if test checks how many times
the method was executed , it's unpredictable if multiple tests are using the same instance of Mock. 
If we want to have only one Mock instance then use this
```
    // services general for all controllers
    @MockBean(reset = MockReset.NONE)
    protected UserService userService;
```
**reset=NONE** will reuser the same instance
