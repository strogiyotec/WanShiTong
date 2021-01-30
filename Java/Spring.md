# Pages
1. [Testing](sping/Testing)
2. [Security](spring/Security)

## References
1. [Spring Boot doc](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-external-config-application-property-files)
2. [Docker](https://spring.io/guides/topicals/spring-boot-docker/)
3. [Core technologies](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-scanning-index)
4. [Docker Java](https://www.youtube.com/watch?v=d7ajT14ENKk)
5. [Docker Spring plugin](https://spring.io/blog/2020/01/27/creating-docker-images-with-spring-boot-2-3-0-m1)
6. [Spring AOT](https://www.jishuwen.com/d/2dkg/zh-tw)
7. [Spring boot test](https://rieckpil.de/spring-boot-test-slices-overview-and-usage/)
8. [Spring pitfals](https://www.nurkiewicz.com/2011/10/spring-pitfalls-proxying.html)

## Improve startup
1. Add these two JVM options `-Xverify:none -XX:TieredStopAtLevel=1`(TieredStopAtLevel)
2. -Dspring.jmx.enabled=false 
3. -Dspring.config.location=classpath:/application.properties


