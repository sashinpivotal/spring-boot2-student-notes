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

### Patterns, Domain Driven Design

- [Microservices Patterns](https://microservices.io/patterns/microservices.html)
- [Handling Distributed Transactions with Saga](https://microservices.io/patterns/data/saga.html)
- [Domain Driven Design Distilled (Vaughn)](https://www.pearson.com/us/higher-education/program/Vernon-Domain-Driven-Design-Distilled/PGM332632.html?tab=contents)
- [Domain Driven Design (Evans)](https://dddcommunity.org/book/evans_2003/)

### Monoliths versus Microservices

The following are complementary, and counter point articles on the topic
of monolith versus microservices that give credence to the
[App Continuum](http://www.appcontinuum.io/) recommendation for
component-based design (for other than trivial systems).

- [Monolith First (Fowler)](https://www.martinfowler.com/bliki/MonolithFirst.html)
- [Don't start with a monolith (Fowler, Tilkov)](https://www.martinfowler.com/articles/dont-start-monolith.html)
- [Anti-Pattern microservices as the goal (Richardson)](https://www.chrisrichardson.net/post/antipatterns/2019/01/14/antipattern-microservices-are-the-goal.html)
- [Microservices and Jars (Martin)](https://blog.cleancoder.com/uncle-bob/2014/09/19/MicroServicesAndJars.html)
- [Clean Microservices Architecture (Martin)](https://blog.cleancoder.com/uncle-bob/2014/10/01/CleanMicroserviceArchitecture.html)

Comments and opinions on [*Don't start with a monolith (Fowler, Tilkov)*](https://www.martinfowler.com/articles/dont-start-monolith.html):

-   The author's criticisms of a monolith are certainly valid if it is a
    [big ball of mud](https://en.wikipedia.org/wiki/Big_ball_of_mud).

-   The [Application Continuum](https://www.appcontinuum.io/) v3 and v4
    stages ensure the appropriate levels of structure and (de)coupling
    that if followed properly, will avoid the
    [big ball of mud](https://en.wikipedia.org/wiki/Big_ball_of_mud).
    If you already have a legacy codebase that is such a mess,
    can you refactor it to a component based build?

-   The title suffix is a bit disturbing:
    *… when your goal is a microservices architecture* - See
    [Anti-Pattern microservices as the goal](https://www.chrisrichardson.net/post/antipatterns/2019/01/14/antipattern-microservices-are-the-goal.html)
    as a counterpoint.
    Unfortunately your enterprise organization may have made this
    decision for you, if so, understand the pitfalls so you can mitigate
    in your processes.

-   There is a subtle but important point the author points out that
    *may* be the exception to
    [Monolith First](https://www.martinfowler.com/bliki/MonolithFirst.html):

    *In my view, the ideal scenario is one where you’re building a second
    version of an existing system.*

    This may be an appropriate strategy large enterprises take to
    re-implement legacy systems that are:
    -   Well known
    -   Where enough information is known to define bounded contexts up
        front
    -   Refactoring a
        [big ball of mud](https://en.wikipedia.org/wiki/Big_ball_of_mud)
        is implausible.

## Continuous Integration and Continuous Delivery

- [Continuous Integration (Fowler)](https://martinfowler.com/articles/continuousIntegration.html)
- [CI Certification (Fowler)](https://martinfowler.com/bliki/ContinuousIntegrationCertification.html)

## Fault Tolerance Patterns

- [Release It! Second Edition](https://pragprog.com/book/mnee2/release-it-second-edition)
- [Chaos Engineering](https://principlesofchaos.org/)
- [Chaos Monkey for Spring Boot apps](https://codecentric.github.io/chaos-monkey-spring-boot/)

## Auto-configuration starter

### Creating our own Spring Boot auto-configuration starter lab

- Clone the [project](https://github.com/sashinpivotal/boot2-autoconfig-helloworld.git) and follow TODO's (TODO 10-16, 20-26, 30-38)

## REST

### Spring MVC and REST

- [Spring Web](https://docs.spring.io/spring/docs/5.1.7.RELEASE/spring-framework-reference/web.html#spring-web)
- [Exception handling in Spring MVC](https://spring.io/blog/2013/11/01/exception-handling-in-spring-mvc)

### RestTemplate

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

```java
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

```java
System.setProperty("java.awt.headless", "false");
```

## Actuator

### Actuator/Prometheus lab

- Follow the [lab instruction](https://github.com/sashinpivotal/spring-boot-actuator-micrometer)

### Liveness and Readiness Probes

New in Spring Boot 2.2.x is the ability to create custom health
endpoints tuned for specific health indicators.

The feature is referred to as [Health Groups](https://docs.spring.io/spring-boot/docs/2.2.7.RELEASE/reference/html/production-ready-features.html#health-groups),
and is useful if you need multiple health endpoints for different
monitoring scenarios.

One such scenaro is the use of "probes",
which is a feature a runtime platform may use to detect the state of
an application instance,
and allow the platform to take action.

Kubernetes,
for example,
has two types of probes:

-   *Readiness probe* intended to indicate an application instance
    is ready for use after its startup or initialization period.

-   *Liveness probe* intended to indicate a running application instance
    may not be able to service requests,
    and allows for the platform to dispose of unhealthy instances.

Spring Boot 2.3.x has first class support for both types of
readiness probes,
which you can read about
[here](https://docs.spring.io/spring-boot/docs/2.3.0.RELEASE/reference/html/production-ready-features.html#production-ready-kubernetes-probes).

## TDD and Spring Boot Testing

-   [TDD best practices presentation](https://www.slideshare.net/axykim00/tdd-practices)

-   Spring Boot TDD-driven testing
    -   Create a new Spring Boot project and follow
        [lab instruction](https://github.com/sashinpivotal/spring-boot-tdd)

    -   Solution project is also included in the above GitHub location

## OAuth2

-   OAuth2 presentations
    - [OAuth2 overview presentation](https://www.slideshare.net/axykim00/spring-security-oauth2)
    - [OAuth2 in cloud native environment presentation (slides 7 to 37)](https://www.slideshare.net/WillTran1/enabling-cloud-native-security-with-oauth2-and-multitenant-uaa?qid=2c77ae8e-b2d5-4319-baad-1cd1eb8fec42&v=&b=&from_search=1)

-   OAuth2 lab
    -   Clone
        [Maven project](https://github.com/sashinpivotal/oauth2-examples)
        and follow TODO's startting from TODO-10

## Spring Cloud Contract

-   [Presentation - only slides 18 to 27](https://www.slideshare.net/MarcinGrzejszczak/consumer-driven-contracts-and-your-microservice-architecture-83680416)
-   [Lab](https://spring.io/guides/gs/contract-rest/):
    Follow instruction under
    "How to complete this guide/To skip the basics, do the following:"
    section

## Tools

- [IntelliJ IDEA default keymap](https://resources.jetbrains.com/storage/products/intellij-idea/docs/IntelliJIDEA_ReferenceCard.pdf)

## Spring

-   Spring Lifecycle changes with Spring 5.+ and Java 11

    -   Java 11 Drops JSR 250 Annotations,
        including `@PostConstruct` & `@PreDestroy`.
        See
        [Java 11 Migration Guide](https://docs.oracle.com/en/java/javase/11/migrate/migration-guide.pdf)
        and
        [Spring Framework docs](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-postconstruct-and-predestroy-annotations)

    -   If using Java 11, you may explicitly import annotations:

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

    -   Or, you may use `@Bean` parameters to specific init and cleanup
        callbacks,
        see
        [Spring Lifecycle Callbacks](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-java-lifecycle-callbacks)
        for more examples.

    -   You may also implement the `InitializingBean` or `DisposableBean`
        on the class you want to provide the lifecycle callbacks,
        but this ties your class to Spring.

### Spring Performance Considerations

- [How Fast is Spring - Blog](https://spring.io/blog/2018/12/12/how-fast-is-spring)
- [How Fast is Spring - Short Deck with Flame graphs](https://presos.dsyer.com/decks/how-fast-is-spring.html)
- [How Fast is Spring - Spring One 2018](https://www.youtube.com/watch?v=97UTDmonq7w)

## Pivotal Applications Services (PAS/PCF)

### Deploy a Spring Boot web application to PCF

1.  Create a free PWS account (if you have not done so yet) from
    [https://run.pivotal.io/](https://run.pivotal.io/)
1.  Download and install `cf` cli from
    [https://docs.cloudfoundry.org/cf-cli/install-go-cli.html](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html)

    - If you are using MacOS, you can use Homebrew

    ```bash
    brew tap cloudfoundry/tap
    brew install cf-cli
    ```

1.  Log in to the PWS

    ```bash
    cf login -a api.run.pivotal.io
    ```

1.  Creat a fat jar from any of your Spring Boot web application

    ```bash
    <core-spring-labfiles/lab> ./gradlew 38-rest-ws-solution:build
    ```

1.  Deploy the application to PCF

    ```bash
    <core-spring-labfiles/lab> cf push my-app -p 38-rest-ws-solution/build/libs/38-rest-ws-solution-5.0.c.RELEASE.jar --random-route
    ```

1.  Display all apps deployed

    ```bash
    <core-spring-labfiles/lab> cf apps
    ```

1.  Access the application from a browser by copying the url of the app
    and place it into a browser's url

1.  You can also manage your applications from PWS App Manager UI by
    going to
    [https://console.run.pivotal.io/](https://console.run.pivotal.io/)

1.  If you enable Actuator in your Spring Boot application,
    you can visualize and manage your Actuator endpoints in App Manager.
    See
    [Using Spring Boot Actuators with Apps Manager](https://docs.pivotal.io/platform/application-service/2-9/console/using-actuators.html)
    for more information.

### OAuth in action in PCF

1.  Observe that every request has `Authorization` request header set
    with [PRIVATE DATA HIDDEN], which represents access token

    ```bash
    <core-spring-labfiles/lab> cf -v app my-app
    ```

1.  Go to <home-directory>/.cf and observe that there is a file called
    `config.json`

1.  Display the contents of `config.json" and observe that it has access
    token

    ```bash
    cat config.json
    ```

1.  Copy the access token (starting from the character right `bearer`
    under `Authorization` field and analyze it via
    [https://jwt.io/](https://jwt.io/)

    - Observe that there are three sections
    - Observe that there are scopes under payload section

### Using MySQL backing servce

-   Clone
    [boot2-mysql-app](https://github.com/sashinpivotal/boot2-mysql-app.git)
    and follow TODO steps

## Holistic Spring Boot Example for greenfield app

The [tracker](https://github.com/billkable/tracker) project is a
holistic Spring Boot example that demonstrates:

-   Basic greenfield Spring Boot MVC REST application
-   Database persistence, using Spring Data JDBC
-   Tags can be used to practice TDD kata to build out a basic Spring
    Boot application
-   Optionally deploy, monitor, and manage the tracker app on PCF

## Spring Boot 1.5 -> 2.0 Migration Considerations

-   [Spring Boot 2.0 Migration guide](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide)

-   If you are running Spring Boot apps accessing Pivotal Cloud Foundry
    Spring Cloud Services,
    you will need to consider Springboot, Spring Cloud, and Pivotal
    Spring Cloud Services version dependencies.
    See
    [Spring Cloud Services Client Dependencies](https://docs.pivotal.io/spring-cloud-services/2-0/common/client-dependencies.html).

## Certification

- [Spring Professional Certification Study Guide](https://d1fto35gcfffzn.cloudfront.net/academy/Spring-Professional-Certification-Study-Guide.pdf)
