## Spring boot Test
> A nice feature of the Spring Test support is that the application context is cached between tests.
> That way, if you have multiple methods in a test case or multiple test cases with the same configuration,
> they incur the cost of starting the application only once.
> You can control the cache by using the @DirtiesContext annotation.


