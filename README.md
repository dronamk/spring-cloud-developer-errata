# Spring Cloud Developer Course Errata

## Logistics and Introduction

### Introduction of Instructors

- Alan McGinlay [amcginlay@pivotal.io](mailto:amcginlay@pivotal.io)
- Bill Kable [bkable@pivotal.io](mailto:bkable@pivotal.io)

### Student Intros

-   How many of you have been working on Java programming more than
    1 year, 3 years, 5 years?
-   How many of you have worked with Spring, Spring Boot?
-   How many of you have worked with PAAS platforms such as Cloud
    Foundry or Kubernetes?
-   What are your objectives for attending this class?

### Pairing

- Pivotal encourages pairing.
- Share knowledge with your pair.
- Share knowledge with your team.

## Workshop Agenda

- Class starts at 9AM and ends at 5PM
- Two 15 minutes breaks, one in the morning and one in the afternoon
- Lunch is 1 hour around noon
- [Course Site](https://courses.education.pivotal.io/c/349802743/)

## Review Prequisites

-   [Review lab setup guide](https://courses.education.pivotal.io/c/349802743/spring-cloud-developer/setup/index.html)
-   Review all students have necessary prereqs.
-   There are places in the course where explicit *nix commands are
    given.
    We will call out alternatives in the errata.
-   Optionally setup a remote for your local code to push, if you want
    to save outside of class.

## Service Discovery

### Application Continuum (Lab)

-   Instead of using "curl" as following:

    ```bash
    curl -i -XPOST -H"Content-Type: application/json" localhost:8084/time-entries/ -d"{\"projectId\": 1, \"userId\": 1, \"date\": \"2015-05-17\", \"hours\": 6}"
    ```
    You can use "httpie" as following:

    ```bash
    http post localhost:8084/time-entries projectId=1 userId=1 date=2015-05-17  hours=6
    ```

    You may also use the supplied Postman collection downloaded with the
    this errata site.
    It is set for localhost, and there are different folders by lab.

-   If you are using Ultimate Edition IntelliJ or STS, you might as well take
    advantage of "Spring Boot Dashboard", which makes running multiple applications
    easier to deal with.

-   If you are a Windows user and decided to use "cmd" terminal window for
    running apps, you might consider to use [ConEMU](https://conemu.github.io/) instead

### Eureka Service Registry (Lab)

-   You are standing up Eureka Server as a Spring Boot application from
    scratch.
    So you will have to create `src/main/java` directory first before
    creating `io.pivotal.pal.tracker.eurekaserver.EurekaServerApp.java`

    Same for creating `application.yml`. You will have to create
    `src/main/resources` directory first before creating `application.yml`
    underneath it.

-   Unlike IntelliJ, Eclipse/STS will not display the
    mult-module project
    in a "easy to understand" hierarchical fashion.
    Instead, it will display all projects in "flat" directory style.
    Just choose correct directory.

-   If you experience the following problem when running Eureka server,
    please use JDK 8 instead of JDK 9+.

    ```bash
    Exception in thread "main" java.lang.NoClassDefFoundError: javax/xml/bind/JAXBException
    ```

    Or add the following dependency to the build.gradle.

    ```groovy
    compile "javax.xml.bind:jaxb-api:2.3.0"
    ```

### Service Discovery Client (Lab)

-   Note that there are three different ways to create discovery client
    -   Using `ServiceLocator` interface and `EurekaServiceLocator`
        implementation class (lab)
    -   Using `DiscoveryClient` (challenge lab exercise)
    -   Using `@LoadBalanced` annotation with `RestTemplate` (in the subsequent lab)

## Fault Tolerance

### Hystrix Isolation Stratgies (Lab)

-   In the JMeter, select HTTO Request, Advanced, Timeouts, and
    set Connect and Response timeout to 1000 and 10000

-   There is an comment error in the “application.properties” file
    of the "timesheets-server". Leave the value as it is.

    ```properties
    # requests that take more than 5 seconds will “fail fast”:
    hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds=2000
    ```

### Hystrix Stats Aggregation (Lab - Optional)

- The command to start RabbitMQ is "rabbitmq-server".

### Hystrix Demo

If you are interested in experimenting with the hystrix demo code, you
can get it [here](./hystrix-demo)

## Config Server and Spring Cloud Bus

-   See the [Spring Cloud Bus documentation](http://cloud.spring.io/spring-cloud-static/spring-cloud-bus/2.0.0.RELEASE/single/spring-cloud-bus.html)
    for how to use and/or customize.

-   See the [sample code](./spring-cloud-bus-demo)
    to demonstrate how to use custom `RemoteApplicationEvent` with Spring
    Cloud Bus and its Event listeners.

### Vault Backend

-   Verison 0.10.x introduces breaking changes.
    Install an older compatible version from [https://releases.hashicorp.com/vault/0.9.6/](https://releases.hashicorp.com/vault/0.9.6/)
    Choose the correct version for your OS, unzip the binary, and copy onto the path (e.g. /usr/local/bin)

### Distributed Updates (lab - Optional)

-   The RabbitMQ management and trace plugins can be installed as
    following on *nix based systems:

    ```bash
    sudo rabbitmq-plugins enable rabbitmq_management
    sudo rabbitmq-plugins enable rabbitmq_tracing
    ```

    Or the following on Windows:

    ```powershell
    rabbitmq-plugins enable rabbitmq_management
    rabbitmq-plugins enable rabbitmq_tracing
    ```

## Additional Topics

### Distributed Trace - Zipkin (Lab - Optional)

-   The document says the following:

    ```bash
    git checkout -b my-zipkin-start zipkin-start
    ```

    In the original lab version you downloaded in course version 1.0.24,
    there was not a "zipkin-start" tag provided.
    The latest updated course has it.
    Make sure you download the
    [latest lab code](https://courses.education.pivotal.io/c/349802635/codebases/spring-cloud-developer-code.zip).

    You will also need to download and run "zipkin.jar" using the
    following instruction (if you were using original lab version)

    ```bash
    curl -sSL https://zipkin.io/quickstart.sh | bash -s
    java -jar zipkin.jar
    ```