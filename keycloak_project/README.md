Yes, using Spring Initialzr is a recommended (though not strictly required) step to create a Spring Boot application. It sets up the correct project structure, dependencies, and configuration files for you.
The dependencies you've selected are:

Spring Web - Provides functionality for building web applications, including RESTful APIs. It includes Spring MVC and uses Apache Tomcat as the default embedded server.
OAuth2 Resource Server - Allows your application to act as an OAuth2 resource server that can validate access tokens and secure your endpoints with OAuth2 authentication.
Spring Security - A powerful authentication and access-control framework that helps secure your application with customizable authentication and authorization mechanisms.
Lombok - A Java library that automatically plugs into your editor and build tools to reduce boilerplate code. It generates getters, setters, constructors, and other common code patterns using annotations.

You could technically create a Spring Boot application manually by setting up all files and dependencies yourself, but Spring Initialzr automates this process, saving you time and ensuring everything is configured correctly from the start.
```yml
# firs thing - 
1. creata a dir under keycloack dir/src/test/java/com/example/keycloak/create a dir called api.
2. In this api dir create - ArticleController.java file.

3. Then in this file under package i wrote - @RestController and this has imported - 
import org.springframework.web.bind.annotation.RestController;

4. now in order to process this when http request send to URL, we will add requestmapping annotation under the last one, then added api part - 
@RequestMapping("/api/articles")

5. now we will impliment a method to send specific message in reposne based on the request send by client.
first get method to retrieve free articles - @GetMapping("basic") [under public class].
then we will define method to handle this get request.
@RequestMapping("/api/articles")
public class ArticleController {
    
    @GetMapping("/basic")
    public String getBasicArticle(){
        return "Free Article";
    }

    @GetMapping("/premium")
    public String getPremiumArticle(){
        return "Premium Article";
    }
}
This way i created artical retrieving api.

# Now starting its security - under keycloack inside src , we created security dir - 
file created called - SecurityConfig.java
we are additing various security measure on the api we develop earlier such as - 
1. JWT verification settings to protect api and session management policies.

to define this class as a config class within spring app, we will add config annotation.
so @Configuration under package, @EnableWebSecurity as well (this enable customize setting and enable security).

- now in security framework, security setting for HTTP request are made through security filter chain class. we need to add Bean annotation to register the object of this class and apply security settings to the entite spring app.
so its @bean under security then a method to configure security. so "public SecurityFilterChain" under Bean.

- Then inside we need to define session creation policy stateless, so no session but rather token.
csrf disable as well.

- Now which request required authentication ? - we want all HTTP request to api need auth. this prevent un authorise user to access paid material =  .authorizeHttpRequests(auth -> auth.anyRequest().authenticated())

- what kind of auth to perform on request ?, it will be JWT auth and we will use oauth2 server to perfrom this. to perform general security setting for JWT (verfyig the issuer and verifying the signature) = 
.oauth2ResourceServer(oauth2 -> oauth2.jwt(Customizer.withDefaults()))

# now we will make sure oath2 perform JWT verification - 
so we need to write application properties for this, src/main/resources then 
"application.properties" now chnage this extension to .yaml.
spring:
  security:
    oath2:
      resourceserver:
        jwt:
first we check if it was issued by intended issur or not and in this case its keycloack server.
issuer is our keycloack server (as we saw from json formatter and validator) = http://localhost:8080/realms/new_realm

now we need to find out if JWT content is trustworthy or not by decrypting and then verifying signature crated by keycloack private key using public by resource server side.
so do jwt_set_uri and then paste keycloak public key url = 
jwt-set-uri: http://localhost:8080/realms/new_realm/protocol/openid-connect/certs
now replace "http://localhost:8080/realms/new_realm" with $ value.

Then port number setting of this resource server as local host 8080 is already used by keycloack server. 
server:
  port: 8081


# we will now actually api using access token, obtained from keycloack to verify if previous config is effective or not.



