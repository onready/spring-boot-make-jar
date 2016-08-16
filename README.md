# Aplicación empaquetada como JAR con Tomcat embebido #

En este proyecto maven vamos a ver como configurar una aplicación con Spring Boot 1.4, que se despliega a través de un Tomcat embebido, es empaquetada como JAR y que utiliza como framework de templating Freemarker 2.3.25.

### ¿Por qué jar y no war? ###

Actualmente muchos desarrollos siguen, o intentan seguir, una arquitectura orientada a microservicios, donde cada uno de ellos es una aplicación en si misma que se comunica con los demás microservicios a través, por ejemplo, de una API REST.
En este contexto se plantea que cada módulo tiene que ser lo más independiente posible para que pueda integrarse fácilmente al entorno, esto incluye también que pueda desplegarse sin necesidad de tener un contenedor de servlets externo.

Cuando creamos una aplicación con Spring boot que utiliza Tomcat embebido para el despliegue, al empaquetarlo como jar, Spring va a autoconfigurarnos algunos aspectos de la aplicación para sólo tener que centrarnos en el desarrollo, a diferencia del empaquetado como war, donde deberemos encargarnos nosotros mismos.

En el proyecto veremos cuales son estas ventajas.

### Configuración del proyecto ###

* Dependencias del POM

```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-web</artifactId>
   </dependency>
```

### Contribution guidelines ###

* Writing tests
* Code review
* Other guidelines

### Who do I talk to? ###

* Repo owner or admin
* Other community or team contact