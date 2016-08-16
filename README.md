# Aplicación empaquetada como JAR con Tomcat embebido #

En este proyecto maven vamos a ver como configurar una aplicación con Spring Boot 1.4, que se despliega a través de un Tomcat embebido, es empaquetada como JAR y que utiliza como framework de templating Freemarker 2.3.25.

### ¿Por qué jar y no war? ###

Actualmente muchos desarrollos siguen, o intentan seguir, una arquitectura orientada a microservicios, donde cada uno de ellos es una aplicación en si misma que se comunica con los demás microservicios a través, por ejemplo, de una API REST.
En este contexto se plantea que cada módulo tiene que ser lo más independiente posible para que pueda integrarse fácilmente al entorno, esto incluye también que pueda desplegarse sin necesidad de tener un contenedor de servlets externo.

Cuando creamos una aplicación con Spring boot que utiliza Tomcat embebido para el despliegue, al empaquetarlo como jar, Spring va a autoconfigurarnos algunos aspectos de la aplicación para sólo tener que centrarnos en el desarrollo, a diferencia del empaquetado como war, donde deberemos encargarnos nosotros mismos.

En el proyecto veremos cuales son estas ventajas.

### Configuración del proyecto MAVEN ###

#### Empaquetar como JAR ####

```xml
<packaging>jar</packaging>
```

#### Spring boot 1.4 ####

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.4.0.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

#### Dependencias ####

* Tomcat embebido

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

Con esta dependencia por defecto podremos usar el Tomcat embebido, por defecto el contenedor de servlets será el Tomcat 8.5.4, en caso de querer usar otro como Jetty o Undertown, se debe excluir de esta dependencia la de Tomcat e incluir la del contenedor elegido.

* Freemarker

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-freemarker</artifactId>
</dependency>
```

Freemarker es el frameework de templating que eligimos para la parte visual. Otras alternativas son Mustache o Thymeleaf. (Velocity fue deprecado a partir de Spring boot 1.4)
Al agregar esta dependencia, el view resolver de MVC tomara, al devolver vistas en los controlers,  el sufijo ".ftl".

La elección de usar templates por sobre JSP se debe a que al empaquetar como JAR, estos últimos poseen algunas limitaciones. Pueden ver más información de esto en estos links:

http://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-developing-web-applications.html
https://blog.stackhunter.com/2014/01/17/10-reasons-to-replace-your-jsps-with-freemarker-templates/

#### Plugin maven spring boot ####

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
```

Este plugin nos va a permitir empaquetar la aplicación como un jar ejecutable, y correrla "in-situ". A demás si no le especificamos una clase principal distinta, el plugin va a buscar, para comenzar la ejecución, la clase que tenga el método:

```java
public static void main(String[] args)
```

En el caso de nuestro proyecto tendremos lo siguiente:

```java
@SpringBootApplication
public class MakejarApplication {

    public static void main(String[] args) {
        SpringApplication.run(MakejarApplication.class, args);
    }

}
```

#### Application properties ####

En el application.properties del proyecto podemos configurar las propiedades del Tomcat embebido. En este ejemplo vamos a ver sólo dos, que son el puerto y el context path donde se levanta la aplicación.

*server.port
*server.context-path

Si no configuramos estas properties el puerto por defecto es el 8080 y el path es "/".

#### Consejo ####

Si ya tenés claro qué significan cada uno de los elementos que configuramos anteriormente, para ahorrar tiempo, podés usar la herramienta [Spring initialzr](https://start.spring.io/) y seleccionar las dependencias que antes especificamos.