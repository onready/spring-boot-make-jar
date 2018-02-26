# Embedded Tomcat JAR packaged application #

On this maven project we are going to see how to configure a spring boot 1.4.3 application, that is being deployed on an embedded tomcat, is packaged as jar and uses Freemarker 2.3.25 as templeating software.

### Why jar and why not to use war? ###

Today many projects follow, or try to follow, a microservice oriented architecture, where every one of them are applications that communicates with the other microservices through, for example, a REST API.
In this context, the challenge is to make every module as much independent as can be so is easily integrable with the enviroment, this includes that it cant deploy itself without the need of having a external servlet container.

When we create an Spring Boot Application that uses an embedded Tomcat for the deployment, on the jar compiling process, Spring is going to autoconfigure some applications aspects so we only have to focus on the product development, instead of the war packagaging, where we have to do that job ourselves.

Another advantage is that, on this way, we avoid the problem of having to sync between the versions of the application and the server configurations, that means, How is the properly server configuration for a determinated version of the code? Because all is now being manage from the application.

We also have to highlight that, on this way we simplify a lot the deployment on cloud enviroments such as Heroku, Microsft Azure or AWS, making easier the continuous integration, because of the facilities that brings those platform of their adaptation to this philosophy of independency of each application.

### Maven project configuration ###

#### JAR packaging ####

```xml
<packaging>jar</packaging>
```

#### Spring boot 1.4.3 ####

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.4.3.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

#### Dependencies ####

* Embedded Tomcat

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

With this dependency by default we are going to be able to use the embedded tomcat, and the Tomcat Servlet container version is, by default, 8.5.6, in case of needing to use another one such as Jetty or Undertown, Tomcat should be excluded of the dependency.

* Freemarker

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-freemarker</artifactId>
</dependency>
```

Freemarker is a templating framework that we chose for the frontend. Another alternatives are Mustache or Thymeleaf. (Velocity was deprecated since Spring Boot 1.4)
When we add this dependency, the MVC view resolver will take, in the time of returning views from the controllers. the ".ftl" suffix.
The ".ftl" files must be on the templates folder, automatically created.

The decision of using templates instead of JSP is because when we package the application as JAR, this last ones have some limitations. You can see mor info on those links:

* http://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-developing-web-applications.html
* https://blog.stackhunter.com/2014/01/17/10-reasons-to-replace-your-jsps-with-freemarker-templates/

#### Maven Spring Boot Plugin ####

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
```

This plugin will allow us to package our application as a executable jar, and run it "in-situ". Also, if we don't specify a principal class, the plugin is going to look after the class that has the following method:
```java
public static void main(String[] args)
```

In the case of this example we are going to have the following:

```java
@SpringBootApplication
public class MakejarApplication {

    public static void main(String[] args) {
        SpringApplication.run(MakejarApplication.class, args);
    }

}
```

#### Application properties ####

On the project application.properties we can configure the Embedded Tomcat properties. On this example we are going to see only two, that are the port and the context path where the application is going to be deployed.

```java
server.port={PORT}
server.context-path={PATH}
```

If we don't configure this properties, the default port is 8080 and the path is "/".
