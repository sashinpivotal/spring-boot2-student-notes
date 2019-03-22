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

- Creating our own Spring Boot auto-configuration starter lab

   - Clone the [project](https://github.com/sashinpivotal/boot2-autoconfig-helloworld.git) and follow TODO's

- OAuth2 

  -  [OAuth2 overview presentation](https://www.slideshare.net/SangShin1/spring4-security-oauth2?qid=2163e6e6-ae99-48b0-afcc-88380b8724d8&v=&b=&from_search=1)
  -  [OAuth2 in cloud native environment presentation (slides 7 to 37)](https://www.slideshare.net/WillTran1/enabling-cloud-native-security-with-oauth2-and-multitenant-uaa?qid=2c77ae8e-b2d5-4319-baad-1cd1eb8fec42&v=&b=&from_search=1)
     
- Actuator/Prometheus lab 

   - Read the [document](https://github.com/sashinpivotal/spring-boot-actuator-micrometer)

- [TDD best practices presentation](https://www.slideshare.net/axykim00/tdd-practices)

- Spring Boot TDD-drive testing 

   - [The solution and lab document](https://github.com/sashinpivotal/spring-boot-tdd)
     
- Spring Cloud Contract 

   - [Presentation - only slides 18 to 27](https://www.slideshare.net/MarcinGrzejszczak/consumer-driven-contracts-and-your-microservice-architecture-83680416)
   - [Lab](https://spring.io/guides/gs/contract-rest/)


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

1. You can also manage your applications from PWS App Manager UI by going to `https://console.run.pivotal.io/` 

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

1. Copy the access token and analyze it via [https://jwt.io/](https://jwt.io/) debugger

