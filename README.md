## Reference materials

In addition to the standard course contents, we might use some extra reference materials below whenever needed:

- [IntelliJ IDEA default keymap](https://resources.jetbrains.com/storage/products/intellij-idea/docs/IntelliJIDEA_ReferenceCard.pdf)

- The 12 Factors App

  - [The 12 Factors presentation](https://content.pivotal.io/slides/the-12-factors-for-building-cloud-native-software)
  - [The 12 Factor App website](https://12factor.net/)
  - [Beyond 12 Factor App blog](https://content.pivotal.io/blog/beyond-the-twelve-factor-app)

- App Continuum

   - [Presentation](http://deck.appcontinuum.io/assets/player/KeynoteDHTMLPlayer.html#0)
   - [Website](http://www.appcontinuum.io/)

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

- __Creating our own Spring Boot auto-configuration starter lab__

   - Clone the [project](https://github.com/sashinpivotal/boot2-autoconfig-helloworld.git) and follow TODO's (TODO 10-16, 20-26, 30-38)

- __OAuth2 presentations__

  -  [OAuth2 overview presentation](https://www.slideshare.net/SangShin1/spring4-security-oauth2?qid=2163e6e6-ae99-48b0-afcc-88380b8724d8&v=&b=&from_search=1)
  -  [OAuth2 in cloud native environment presentation (slides 7 to 37)](https://www.slideshare.net/WillTran1/enabling-cloud-native-security-with-oauth2-and-multitenant-uaa?qid=2c77ae8e-b2d5-4319-baad-1cd1eb8fec42&v=&b=&from_search=1)

- OAuth2 lab

  - Clone [Maven project](https://github.com/sashinpivotal/oauth2-examples) and follow TODO's startting from TODO-10

- __Actuator/Prometheus lab__

   - Follow the [lab instruction](https://github.com/sashinpivotal/spring-boot-actuator-micrometer)

- [TDD best practices presentation](https://www.slideshare.net/axykim00/tdd-practices)

- __Spring Boot TDD-driven testing__

   - Create a new Spring Boot project and follow [lab instruction](https://github.com/sashinpivotal/spring-boot-tdd)
   - Solution project is also included in the
     above GitHub location

- __Spring Cloud Contract__

   - [Presentation - only slides 18 to 27](https://www.slideshare.net/MarcinGrzejszczak/consumer-driven-contracts-and-your-microservice-architecture-83680416)
   - [Lab](https://spring.io/guides/gs/contract-rest/)
    : Follow instruction under "How to complete this guide/To skip the basics, do the following:" section

- __Spring MVC and REST__

   - [Spring Web](https://docs.spring.io/spring/docs/5.1.7.RELEASE/spring-framework-reference/web.html#spring-web)
   - [Exception handling in Spring MVC](https://spring.io/blog/2013/11/01/exception-handling-in-spring-mvc)

## How to deploy an app to PCF

### Deploy a Spring Boot web application

1. Create a free PWS account (if you have not done so yet) from [https://run.pivotal.io/](https://run.pivotal.io/)
1. Download and install `cf` cli from [https://docs.cloudfoundry.org/cf-cli/install-go-cli.html](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html)

    - If you are using MacOS, you can use Homebrew

    ```
    brew tap cloudfoundry/tap
    brew install cf-cli
    ```

1. Log in to the PWS

   ```
   cf login -a api.run.pivotal.io
   ```

1. Creat a fat jar from any of your Spring Boot web application

   ```
   <core-spring-labfiles/lab> ./gradlew 38-rest-ws-solution:build
   ```

1. Deploy the application to PCF

   ```
   <core-spring-labfiles/lab> cf push my-app -p 38-rest-ws-solution/build/libs/38-rest-ws-solution-5.0.c.RELEASE.jar --random-route
   ```

1. Display all apps deployed

   ```
   <core-spring-labfiles/lab> cf apps
   ```

1. Access the application from a browser by copying the url of the app and place it into a browser's url

1. You can also manage your applications from PWS App Manager UI by going to [https://console.run.pivotal.io/](https://console.run.pivotal.io/)

1. If you enable Actuator in your Spring Boot application,
   you can visualize and manage your Actuator endpoints in App Manager.
   See [Using Spring Boot Actuators with Apps Manager](https://docs.pivotal.io/pivotalcf/2-5/console/using-actuators.html) for more information.

### OAuth in action

1. Observe that every request has `Authorization` request header set with [PRIVATE DATA HIDDEN], which represents access token

   ```
   <core-spring-labfiles/lab> cf -v app my-app
   ```

1. Go to <home-directory>/.cf and observe that there is a file called `config.json`

1. Display the contents of `config.json" and observe that it has access token

   ```
   cat config.json
   ```

1. Copy the access token (starting from the character right `bearer` under `Authorization` field and analyze it via [https://jwt.io/](https://jwt.io/)

   - Observe that there are three sections
   - Observe that there are scopes under payload section

## Spring Performance Considerations

- [How Fast is Spring - Blog](https://spring.io/blog/2018/12/12/how-fast-is-spring)
- [How Fast is Spring - Short Deck with Flame graphs](https://presos.dsyer.com/decks/how-fast-is-spring.html)
- [How Fast is Spring - Spring One 2018](https://www.youtube.com/watch?v=97UTDmonq7w)


## Spring Boot 1.5 -> 2.0 Migration Considerations

- [Spring Boot 2.0 Migration guide](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide)

-  If you are running Spring Boot apps accessing Pivotal Cloud Foundry
   Spring Cloud Services,
   you will need to consider Springboot, Spring Cloud, and Pivotal
   Spring Cloud Services version dependencies.
   See
   [Spring Cloud Services Client Dependencies](https://docs.pivotal.io/spring-cloud-services/2-0/common/client-dependencies.html).

## XML and Java Configuration

-   [XML Config example](https://howtodoinjava.com/spring5/core/applicationcontext-xml-config-example/)
-   [Annotation vs XML](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-annotation-config)

## Rest Template Builder

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

## Microservices & Fault Tolerance Patterns

- [Microservices Patterns](https://microservices.io/patterns/microservices.html)
- [Release It! Second Edition](https://pragprog.com/book/mnee2/release-it-second-edition)
- [Chaos Engineering](https://principlesofchaos.org/)
- [Chaos Monkey for Spring Boot apps](https://codecentric.github.io/chaos-monkey-spring-boot/)

## Certification

- [Spring Professional Certification Study Guide](https://d1fto35gcfffzn.cloudfront.net/academy/Spring-Professional-Certification-Study-Guide.pdf)