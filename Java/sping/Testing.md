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


