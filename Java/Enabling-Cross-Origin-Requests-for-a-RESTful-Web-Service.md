# Enabling Cross Origin Requests for a RESTful Web Service

> 转译自：https://spring.io/guides/gs/rest-service-cors/

This guide walks you through the process of creating a “Hello, World” RESTful web service with Spring that includes headers for Cross-Origin Resource Sharing (CORS) in the response. You can find more information about Spring CORS support in this blog post.

## What You Will Build

You will build a service that accepts HTTP GET requests at http://localhost:8080/greeting and responds with a JSON representation of a greeting, as the following listing shows:

```
{"id":1,"content":"Hello, World!"}
```

You can customize the greeting with an optional name parameter in the query string, as the following listing shows:

```
http://localhost:8080/greeting?name=User
```

The name parameter value overrides the default value of World and is reflected in the response, as the following listing shows:

```
{"id":1,"content":"Hello, User!"}
```

This service differs slightly from the one described in Building a RESTful Web Service, in that it uses Spring Framework CORS support to add the relevant CORS response headers.

## What You Need

    About 15 minutes

    A favorite text editor or IDE

    JDK 1.8 or later

    Gradle 4+ or Maven 3.2+

    You can also import the code straight into your IDE:

        Spring Tool Suite (STS)

        IntelliJ IDEA

## How to complete this guide

Like most Spring Getting Started guides, you can start from scratch and complete each step or you can bypass basic setup steps that are already familiar to you. Either way, you end up with working code.

To start from scratch, move on to Starting with Spring Initializr.

To skip the basics, do the following:

- Download and unzip the source repository for this guide, or clone it using Git: git clone https://github.com/spring-guides/gs-rest-service-cors.git

- cd into gs-rest-service-cors/initial

- Jump ahead to Create a Resource Representation Class.

**When you finish**, you can check your results against the code in gs-rest-service-cors/complete.

## Starting with Spring Initializr

For all Spring applications, you should start with the Spring Initializr. The Initializr offers a fast way to pull in all the dependencies you need for an application and does a lot of the setup for you. This example needs only the Spring Web dependency. The following image shows the Initializr set up for this sample project:

（图片略）

> The preceding image shows the Initializr with Maven chosen as the build tool. You can also use Gradle. It also shows values of com.example and rest-service-cors as the Group and Artifact, respectively. You will use those values throughout the rest of this sample.

The following listing shows the pom.xml file that is created when you choose Maven:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.2.0.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>rest-service-cors</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>rest-service-cors</name>
	<description>Demo project for Spring Boot</description>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```

The following listing shows the build.gradle file that is created when you choose Gradle:

```
plugins {
	id 'org.springframework.boot' version '2.2.0.RELEASE'
	id 'io.spring.dependency-management' version '1.0.8.RELEASE'
	id 'java'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-web'
	testImplementation('org.springframework.boot:spring-boot-starter-test') {
		exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
	}
}

test {
	useJUnitPlatform()
}
```

### Adding the httpclient Dependency

The tests (in complete/src/test/java/com/example/restservicecors/GreetingIntegrationTests.java) require the Apache httpclient library.

To add the Apache httpclient library to Maven, add the following dependency:

```
<dependency>
  <groupId>org.apache.httpcomponents</groupId>
  <artifactId>httpclient</artifactId>
  <scope>test</scope>
</dependency>
```

The following listing shows the finished pom.xml file:

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.2.0.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>rest-service-cors</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>rest-service-cors</name>
	<description>Demo project for Spring Boot</description>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.apache.httpcomponents</groupId>
			<artifactId>httpclient</artifactId>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
			<exclusions>
				<exclusion>
					<groupId>org.junit.vintage</groupId>
					<artifactId>junit-vintage-engine</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```

To add the Apache httpclient library to Gradle, add the following dependency:

```
testImplementation 'org.apache.httpcomponents:httpclient'
```

The following listing shows the finished build.gradle file:

```
plugins {
	id 'org.springframework.boot' version '2.2.0.RELEASE'
	id 'io.spring.dependency-management' version '1.0.8.RELEASE'
	id 'java'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-web'
	testImplementation 'org.apache.httpcomponents:httpclient'
	testImplementation('org.springframework.boot:spring-boot-starter-test') {
		exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
	}
}

test {
	useJUnitPlatform()
}
```

## Create a Resource Representation Class

Now that you have set up the project and build system, you can create your web service.

Begin the process by thinking about service interactions.

The service will handle GET requests to /greeting, optionally with a name parameter in the query string. The GET request should return a 200 OK response with JSON in the body to represent a greeting. It should resemble the following listing:

```
{
    "id": 1,
    "content": "Hello, World!"
}
```

The id field is a unique identifier for the greeting, and content is the textual representation of the greeting.

To model the greeting representation, create a resource representation class. Provide a plain old Java object with fields, constructors, and accessors for the id and content data, as the following listing (from src/main/java/com/example/restservicecors/Greeting.java) shows:

```
package com.example.restservicecors;

public class Greeting {

	private final long id;
	private final String content;

	public Greeting() {
		this.id = -1;
		this.content = "";
	}

	public Greeting(long id, String content) {
		this.id = id;
		this.content = content;
	}

	public long getId() {
		return id;
	}

	public String getContent() {
		return content;
	}
}
```

> Spring uses the Jackson JSON library to automatically marshal instances of type Greeting into JSON.

## Create a Resource Controller

In Spring’s approach to building RESTful web services, HTTP requests are handled by a controller. These components are easily identified by the @Controller annotation, and the GreetingController shown in the following listing (from src/main/java/com/example/restservicecors/GreetingController.java) handles GET requests for /greeting by returning a new instance of the Greeting class:

```java
package com.example.restservicecors;

import java.util.concurrent.atomic.AtomicLong;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {

	private static final String template = "Hello, %s!";
	private final AtomicLong counter = new AtomicLong();

	@GetMapping("/greeting")
	public Greeting greeting(@RequestParam(required=false, defaultValue="World") String name) {
		System.out.println("==== in greeting ====");
		return new Greeting(counter.incrementAndGet(), String.format(template, name));
	}

}
```

This controller is concise and simple, but there is plenty going on under the hood. We break it down step by step.

The @RequestMapping annotation ensures that HTTP requests to /greeting are mapped to the greeting() method.

> The preceding example uses the @GetMapping annotation, which acts as a shortcut for @RequestMapping(method = RequestMethod.GET).

@RequestParam binds the value of the name query string parameter into the name parameter of the greeting() method. This query string parameter is not required. If it is absent in the request, the defaultValue of World is used.

The implementation of the method body creates and returns a new Greeting object, with the value of the id attribute based on the next value from the counter and the value of the content based on the query parameter or the default value. It also formats the given name by using the greeting template.

A key difference between a traditional MVC controller and the RESTful web service controller shown earlier is the way that the HTTP response body is created. Rather than relying on a view technology to perform server-side rendering of the greeting data to HTML, this RESTful web service controller populates and returns a Greeting object. The object data is written directly to the HTTP response as JSON.

To accomplish this, the @ResponseBody annotation on the greeting() method tells Spring MVC that it does not need to render the greeting object through a server-side view layer. Instead, the returned greeting object is the response body and should be written out directly.

The Greeting object must be converted to JSON. Thanks to Spring’s HTTP message converter support, you need not do this conversion manually. Because Jackson is on the classpath, Spring’s MappingJackson2HttpMessageConverter is automatically chosen to convert the Greeting instance to JSON.

## Enabling CORS

You can enable cross-origin resource sharing (CORS) from either in individual controllers or globally. The following topics describe how to do so:

- Controller Method CORS Configuration

- Global CORS configuration

### Controller Method CORS Configuration

So that the RESTful web service will include CORS access control headers in its response, you have to add a @CrossOrigin annotation to the handler method, as the following listing (from src/main/java/com/example/restservicecors/GreetingController.java) shows:

```java
	@CrossOrigin(origins = "http://localhost:9000")
	@GetMapping("/greeting")
	public Greeting greeting(@RequestParam(required=false, defaultValue="World") String name) {
		System.out.println("==== in greeting ====");
		return new Greeting(counter.incrementAndGet(), String.format(template, name));
	}
```

This @CrossOrigin annotation enables cross-origin resource sharing only for this specific method. By default, its allows all origins, all headers, and the HTTP methods specified in the @RequestMapping annotation. Also, a maxAge of 30 minutes is used. You can customize this behavior by specifying the value of one of the following annotation attributes:

- origins

- methods

- allowedHeaders

- exposedHeaders

- allowCredentials

- maxAge.

In this example, we allow only http://localhost:9000 to send cross-origin requests.

> You can also add the @CrossOrigin annotation at the controller class level as well, to enable CORS on all handler methods of this class.

### Global CORS configuration

In addition (or as an alternative) to fine-grained annotation-based configuration, you can define some global CORS configuration as well. This is similar to using a Filter but can be declared within Spring MVC and combined with fine-grained @CrossOrigin configuration. By default, all origins and GET, HEAD, and POST methods are allowed.

The following listing (from src/main/java/com/example/restservicecors/GreetingController.java) shows the greetingWithJavaconfig method in the GreetingController class:

```java
	@GetMapping("/greeting-javaconfig")
	public Greeting greetingWithJavaconfig(@RequestParam(required=false, defaultValue="World") String name) {
		System.out.println("==== in greeting ====");
		return new Greeting(counter.incrementAndGet(), String.format(template, name));
	}
```

> The difference between the greetingWithJavaconfig method and the greeting method (used in the controller-level CORS configuration) is the route (/greeting-javaconfig rather than /greeting) and the presence of the @CrossOrigin origin.

The following listing (from src/main/java/com/example/restservicecors/RestServiceCorsApplication.java) shows how to add CORS mapping in the application class:

```java
	public WebMvcConfigurer corsConfigurer() {
		return new WebMvcConfigurer() {
			@Override
			public void addCorsMappings(CorsRegistry registry) {
				registry.addMapping("/greeting-javaconfig").allowedOrigins("http://localhost:9000");
			}
		};
	}
```

You can easily change any properties (such as allowedOrigins in the example), as well as apply this CORS configuration to a specific path pattern.

> You can combine global- and controller-level CORS configuration.

## Creating the Application Class

The Spring Initializr creates a bare-bones application class for you. The following listing (from initial/src/main/java/com/example/restservicecors/RestServiceCorsApplication.java) shows that initial class:

```java
package com.example.restservicecors;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class RestServiceCorsApplication {

	public static void main(String[] args) {
		SpringApplication.run(RestServiceCorsApplication.class, args);
	}

}
```

You need to add a method to configure how to handle cross-origin resource sharing. The following listing (from complete/src/main/java/com/example/restservicecors/RestServiceCorsApplication.java) shows how to do so:

```java
	@Bean
	public WebMvcConfigurer corsConfigurer() {
		return new WebMvcConfigurer() {
			@Override
			public void addCorsMappings(CorsRegistry registry) {
				registry.addMapping("/greeting-javaconfig").allowedOrigins("http://localhost:9000");
			}
		};
	}
```

The following listing shows the completed application class:

```java
package com.example.restservicecors;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@SpringBootApplication
public class RestServiceCorsApplication {

	public static void main(String[] args) {
		SpringApplication.run(RestServiceCorsApplication.class, args);
	}

	@Bean
	public WebMvcConfigurer corsConfigurer() {
		return new WebMvcConfigurer() {
			@Override
			public void addCorsMappings(CorsRegistry registry) {
				registry.addMapping("/greeting-javaconfig").allowedOrigins("http://localhost:9000");
			}
		};
	}

}
```

@SpringBootApplication is a convenience annotation that adds all of the following:

- @Configuration: Tags the class as a source of bean definitions for the application context.

- @EnableAutoConfiguration: Tells Spring Boot to start adding beans based on classpath settings, other beans, and various property settings. For example, if spring-webmvc is on the classpath, this annotation flags the application as a web application and activates key behaviors, such as setting up a DispatcherServlet.

- @ComponentScan: Tells Spring to look for other components, configurations, and services in the com/example package, letting it find the controllers.

The main() method uses Spring Boot’s SpringApplication.run() method to launch an application. Did you notice that there was not a single line of XML? There is no web.xml file, either. This web application is 100% pure Java and you did not have to deal with configuring any plumbing or infrastructure.

### Build an executable JAR

You can run the application from the command line with Gradle or Maven. You can also build a single executable JAR file that contains all the necessary dependencies, classes, and resources and run that. Building an executable jar so makes it easy to ship, version, and deploy the service as an application throughout the development lifecycle, across different environments, and so forth.

If you use Gradle, you can run the application by using ./gradlew bootRun. Alternatively, you can build the JAR file by using ./gradlew build and then run the JAR file, as follows:

```
java -jar build/libs/gs-rest-service-cors-0.1.0.jar
```

If you use Maven, you can run the application by using ./mvnw spring-boot:run. Alternatively, you can build the JAR file with ./mvnw clean package and then run the JAR file, as follows:

```
java -jar target/gs-rest-service-cors-0.1.0.jar
```

> The steps described here create a runnable JAR. You can also build a classic WAR file.

Logging output is displayed. The service should be up and running within a few seconds.

## Test the service

Now that the service is up, visit http://localhost:8080/greeting, where you should see:

```
{"id":1,"content":"Hello, World!"}
```

Provide a name query string parameter by visiting http://localhost:8080/greeting?name=User. The value of the content attribute changes from Hello, World! to Hello User!, as the following listing shows:

```
{"id":2,"content":"Hello, User!"}
```

This change demonstrates that the @RequestParam arrangement in GreetingController works as expected. The name parameter has been given a default value of World but can always be explicitly overridden through the query string.

Also, the id attribute has changed from 1 to 2. This proves that you are working against the same GreetingController instance across multiple requests and that its counter field is being incremented on each call, as expected.

Now you can test that the CORS headers are in place and allow a Javascript client from another origin to access the service. To do so, you need to create a Javascript client to consume the service. The following listing shows such a client:

First, create a simple Javascript file named hello.js (from complete/public/hello.js) with the following content:

```js
$(document).ready(function() {
  $.ajax({
    url: "http://localhost:8080/greeting"
  }).then(function(data, status, jqxhr) {
    $(".greeting-id").append(data.id);
    $(".greeting-content").append(data.content);
    console.log(jqxhr);
  });
});
```

This script uses jQuery to consume the REST service at http://localhost:8080/greeting. It is loaded by index.html, as the following listing (from complete/public/index.html) shows:

```xml
<!DOCTYPE html>
<html>
    <head>
        <title>Hello CORS</title>
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"></script>
        <script src="hello.js"></script>
    </head>

    <body>
        <div>
            <p class="greeting-id">The ID is </p>
            <p class="greeting-content">The content is </p>
        </div>
    </body>
</html>
```

> This is essentially the REST client created in Consuming a RESTful Web Service with jQuery, modified slightly to consume the service when it runs on localhost at port 8080. See that guide for more details on how this client was developed.

Because the REST service is already running on localhost at port 8080, you need to be sure to start the client from another server or port. Doing so not only avoids a collision between the two applications but also ensures that the client code is served from a different origin than the service. To start the client running on localhost at port 9000, run the following Maven command:

```
./mvnw spring-boot:run -Dserver.port=9000
```

Once the client starts, open http://localhost:9000 in your browser, where you should see the following:

（图）

If the service response includes the CORS headers, then the ID and content are rendered into the page. But if the CORS headers are missing (or insufficiently defined for the client), the browser fails the request and the values are not rendered into the DOM. In that case, you should see the following:

（图）

## Summary

Congratulations! You have just developed a RESTful web service that includes Cross-Origin Resource Sharing with Spring.

## See Also

The following guides may also be helpful:

- Building a RESTful Web Service

- Building a Hypermedia-Driven RESTful Web Service

- Creating API Documentation with Restdocs

- Accessing GemFire Data with REST

- Accessing MongoDB Data with REST

- Accessing data with MySQL

- Accessing JPA Data with REST

- Accessing Neo4j Data with REST

- Consuming a RESTful Web Service

- Consuming a RESTful Web Service with AngularJS

- Consuming a RESTful Web Service with jQuery

- Consuming a RESTful Web Service with rest.js

- Securing a Web Application

- Building REST services with Spring

- React.js and Spring Data REST

- Building an Application with Spring Boot

Want to write a new guide or contribute to an existing one? Check out our contribution guidelines.

> All guides are released with an ASLv2 license for the code, and an Attribution, NoDerivatives creative commons license for the writing.
