# Reference materials

In addition to the standard course contents, we might use some extra reference materials below whenever needed:


## Microservices

### The 12 Factors App

  - [The 12 Factors presentation](https://content.pivotal.io/slides/the-12-factors-for-building-cloud-native-software)
  - [The 12 Factor App website](https://12factor.net/)
  - [Beyond 12 Factor App blog](https://content.pivotal.io/blog/beyond-the-twelve-factor-app)

### App Continuum

   - [Presentation](http://deck.appcontinuum.io/assets/player/KeynoteDHTMLPlayer.html#0)
   - [Website](http://www.appcontinuum.io/)
   - [Sample application](https://github.com/platform-acceleration-lab/pal-tracker-distributed)

      
## REST

### Spring MVC and REST

   - [Spring Web](https://docs.spring.io/spring/docs/5.1.7.RELEASE/spring-framework-reference/web.html#spring-web)
   - [Exception handling in Spring MVC](https://spring.io/blog/2013/11/01/exception-handling-in-spring-mvc)

###  RestTemplate

-   [Javadoc](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/web/client/RestTemplateBuilder.html)
-   [RestTemplate Customization](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-resttemplate.html#boot-features-resttemplate-customization)
-   [Tutorial](https://www.baeldung.com/spring-rest-template-builder)
-   Example of a Spring Cloud Load Balanced Rest Template customized with
    timeout:

    ```java
        /**
         * RestTemplate Builder
         *
         * Following example sets connect timeout of 500ms,
         * where if client cannot acquire open connection on a socket,
         * it will time out.
         *
         * Operations that timeout on connect events may be retried without
         * regard to Idempotence.
         *
         * The example also shows how to configure the read timeout to 2
         * seconds.
         * This protects the calling client from long running calls in a
         * downstream server/producer.
         *
         * Retries may only be executed across read timeouts if the down
         * stream operation is idempotent,
         * use with care.
         *
         * @return RestTemplate
         */
        @LoadBalanced
        @Bean
        public RestTemplate restTemplate() {
            return new RestTemplateBuilder()
                    .setConnectTimeout(Duration.ofMillis(500L))
                    .setReadTimeout(Duration.ofMillis(2000L))
                    .build();
        }
    ```

## How to run HSQLDB Client

- Add the following configuration class

```
import org.hsqldb.util.DatabaseManagerSwing;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.core.JdbcTemplate;

import javax.annotation.PostConstruct;
import javax.sql.DataSource;

@Configuration
public class HsqldbConfig {

    @Autowired
    DataSource dataSource;

    @Bean
    public JdbcTemplate getJdbcTemplate(){
        return new JdbcTemplate(dataSource);
    }

    //default username : sa, password : ''
    @PostConstruct
    public void getDbManager(){
        DatabaseManagerSwing.main(
                new String[] { "--url", "jdbc:hsqldb:mem:testdb", "--user", "sa", "--password", ""});
    }
}
```

- Turn off the "java.awt.headless"

```
	System.setProperty("java.awt.headless", "false");
```

## Actuator

### Actuator/Prometheus lab

   - Follow the [lab instruction](https://github.com/sashinpivotal/spring-boot-actuator-micrometer)



## Spring Boot Testing

- [TDD best practices presentation](https://www.slideshare.net/axykim00/tdd-practices)

- Spring Boot TDD-driven testing

   - Create a new Spring Boot project and follow [lab instruction](https://github.com/sashinpivotal/spring-boot-tdd)
   - Solution project is also included in the
     above GitHub location




## OAuth2

- OAuth2 presentations

  -  [OAuth2 overview presentation](https://www.slideshare.net/axykim00/spring-security-oauth2)
  -  [OAuth2 in cloud native environment presentation (slides 7 to 37)](https://www.slideshare.net/WillTran1/enabling-cloud-native-security-with-oauth2-and-multitenant-uaa?qid=2c77ae8e-b2d5-4319-baad-1cd1eb8fec42&v=&b=&from_search=1)

- OAuth2 lab

  - Clone [Maven project](https://github.com/sashinpivotal/oauth2-examples) and follow TODO's startting from TODO-10
  

## Spring Cloud Contract

   - [Presentation - only slides 18 to 27](https://www.slideshare.net/MarcinGrzejszczak/consumer-driven-contracts-and-your-microservice-architecture-83680416)
   - [Lab](https://spring.io/guides/gs/contract-rest/)
    : Follow instruction under "How to complete this guide/To skip the basics, do the following:" section

  
## Tools

- [IntelliJ IDEA default keymap](https://resources.jetbrains.com/storage/products/intellij-idea/docs/IntelliJIDEA_ReferenceCard.pdf)

## Spring related

- Spring Lifecycle changes with Spring 5.+ and Java 11

   -  Java 11 Drops JSR 250 Annotations,
      including `@PostConstruct` & `@PreDestroy`.
      See [Java 11 Migration Guide](https://docs.oracle.com/en/java/javase/11/migrate/migration-guide.pdf)
      and
      [Spring Framework docs](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-postconstruct-and-predestroy-annotations)
   -  If using Java 11, you may explicitly import annotations:

      ```gradle
      implementation 'javax.annotation:javax.annotation-api:1.3.2'
      ```

      ```maven
      <dependency>
         <groupId>javax.annotation</groupId>
         <artifactId>javax.annotation-api</artifactId>
         <version>1.3.2</version>
      </dependency>
      ```

   -  Or, you may use `@Bean` parameters to specific init and cleanup
      callbacks,
      see
      [Spring Lifecycle Callbacks](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-java-lifecycle-callbacks)
      for more examples.

   -  You may also implement the `InitializingBean` or `DisposableBean`
      on the class you want to provide the lifecycle callbacks,
      but this ties your class to Spring.



# Spring Performance Considerations

- [How Fast is Spring - Blog](https://spring.io/blog/2018/12/12/how-fast-is-spring)
- [How Fast is Spring - Short Deck with Flame graphs](https://presos.dsyer.com/decks/how-fast-is-spring.html)
- [How Fast is Spring - Spring One 2018](https://www.youtube.com/watch?v=97UTDmonq7w)


# Spring Boot 1.5 -> 2.0 Migration Considerations

- [Spring Boot 2.0 Migration guide](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide)

-  If you are running Spring Boot apps accessing Pivotal Cloud Foundry
   Spring Cloud Services,
   you will need to consider Springboot, Spring Cloud, and Pivotal
   Spring Cloud Services version dependencies.
   See
   [Spring Cloud Services Client Dependencies](https://docs.pivotal.io/spring-cloud-services/2-0/common/client-dependencies.html).



# Microservices & Fault Tolerance Patterns

- [Microservices Patterns](https://microservices.io/patterns/microservices.html)
- [Release It! Second Edition](https://pragprog.com/book/mnee2/release-it-second-edition)
- [Chaos Engineering](https://principlesofchaos.org/)
- [Chaos Monkey for Spring Boot apps](https://codecentric.github.io/chaos-monkey-spring-boot/)

# Certification

- [Spring Professional Certification Study Guide](https://d1fto35gcfffzn.cloudfront.net/academy/Spring-Professional-Certification-Study-Guide.pdf)