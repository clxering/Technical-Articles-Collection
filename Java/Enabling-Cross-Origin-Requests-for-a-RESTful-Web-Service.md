# Enabling Cross Origin Requests for a RESTful Web Service

**为 RESTful Web 服务启用跨域请求**

> 转译自：https://spring.io/guides/gs/rest-service-cors/

This guide walks you through the process of creating a “Hello, World” RESTful web service with Spring that includes headers for Cross-Origin Resource Sharing (CORS) in the response. You can find more information about Spring CORS support in this blog post.

本指南指导你使用 Spring 创建「Hello, World」RESTful Web 服务，包括响应跨域资源共享（CORS）头。你可以在这篇博客文章中找到更多关于 Spring 支持 CORS 的信息。

## What You Will Build

**你将创建什么项目**

You will build a service that accepts HTTP GET requests at http://localhost:8080/greeting and responds with a JSON representation of a greeting, as the following listing shows:

你将建立一个服务器端，它能够接受一个 HTTP GET 请求 http://localhost:8080/greeting，并且响应一个 JSON 格式的数据，如下所示：

```java
{"id":1,"content":"Hello, World!"}
```

You can customize the greeting with an optional name parameter in the query string, as the following listing shows:

你可以在这个查询中为这个问候语定义一个可选的 name 参数，如下所示：

```
http://localhost:8080/greeting?name=User
```

The name parameter value overrides the default value of World and is reflected in the response, as the following listing shows:

name 参数值将覆盖默认值「World」并体现在响应中，如下所示：

```java
{"id":1,"content":"Hello, User!"}
```

This service differs slightly from the one described in Building a RESTful Web Service, in that it uses Spring Framework CORS support to add the relevant CORS response headers.

这个服务与其他构建 RESTful Web Service 中描述的服务略有不同，因为它使用 Spring Framework CORS 支持来添加相关的 CORS 响应头。

## What You Need

**需要如下环境**

- About 15 minutes
- A favorite text editor or IDE
- JDK 1.8 or later
- Gradle 4+ or Maven 3.2+
- You can also import the code straight into your IDE:
  - Spring Tool Suite (STS)
  - IntelliJ IDEA

## How to complete this guide

**如何完成这个指南**

Like most Spring Getting Started guides, you can start from scratch and complete each step or you can bypass basic setup steps that are already familiar to you. Either way, you end up with working code.

就像大多数 Spring 入门指南一样，你可以从头开始并完成每个步骤，也可以跳过您已经熟悉的基本设置步骤。无论哪种方式，您最终得到的都是能够工作的代码。

To **start from scratch**, move on to Starting with Spring Initializr.

要从头开始，请继续从 Spring Initializr 开始。

To **skip the basics**, do the following:

要跳过这些基本步骤，请执行以下步骤:

- Download and unzip the source repository for this guide, or clone it using Git: `git clone https://github.com/spring-guides/gs-rest-service-cors.git`

下载并解压本指南的源码存储库，或者使用 Git 克隆它：`git clone https://github.com/spring-guides/gs-rest-service-cors.git`

- cd into gs-rest-service-cors/initial

进入 `gs-rest-service-cors/initial` 目录

- Jump ahead to Create a Resource Representation Class.

跳到 Create a Resource Representation Class 前面。

**When you finish**, you can check your results against the code in `gs-rest-service-cors/complete`.

完成后，可以根据 `gs-rest-service-cors/complete` 目录中的代码检查结果。

## Starting with Spring Initializr

**从 Spring Initializr 开始**

For all Spring applications, you should start with the Spring Initializr. The Initializr offers a fast way to pull in all the dependencies you need for an application and does a lot of the setup for you. This example needs only the Spring Web dependency. The following image shows the Initializr set up for this sample project:

对于所有 Spring 应用程序，应该从 Spring Initializr 开始。Initializr 提供了一种快捷方法来获取应用程序所需的所有依赖项，并做了大量的设置工作。这个例子只需要 Spring Web 依赖项。下图显示了为这个示例项目设置的 Initializr：

（原文该处为 Spring Initializr 页面图片，略）

> The preceding image shows the Initializr with Maven chosen as the build tool. You can also use Gradle. It also shows values of com.example and rest-service-cors as the Group and Artifact, respectively. You will use those values throughout the rest of this sample.

前面的图片显示了项目以 Maven 作为 Initializr 的构建工具。你也可以使用 Gradle。它还显示了 com.example 的值。将 rest-service-cors 分别作为 Group 和 Artifact 的值。在本示例的其余部分中会使用这些值。

The following listing shows the pom.xml file that is created when you choose Maven:

当选择 Maven 作为构建工具时，生成的 pom.xml 文件如下所示：

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

当选择 Gradle 作为构建工具时，生成的 build.gradle 文件如下所示：

```groovy
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

## Adding the httpclient Dependency

**增加 httpclient 依赖**

The tests (in complete/src/test/java/com/example/restservicecors/GreetingIntegrationTests.java) require the Apache httpclient library.

单元测试（路径为 `complete/src/test/java/com/example/restservicecors/GreetingIntegrationTests.java`）需要 Apache httpclient 库。

To add the Apache httpclient library to Maven, add the following dependency:

在 Maven 增加 Apache httpclient 库的依赖：

```xml
<dependency>
  <groupId>org.apache.httpcomponents</groupId>
  <artifactId>httpclient</artifactId>
  <scope>test</scope>
</dependency>
```

The following listing shows the finished pom.xml file:

最终的 pom.xml 文件如下所示：

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

在 Gradle 增加 Apache httpclient 库的依赖：

```groovy
testImplementation 'org.apache.httpcomponents:httpclient'
```

The following listing shows the finished build.gradle file:

最终的 build.gradle 文件如下所示：

```groovy
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

**创建资源表示类**

Now that you have set up the project and build system, you can create your web service.

现在已经建立了项目和构建系统，可以创建 web 服务了。

Begin the process by thinking about service interactions.

在这个过程中考虑服务交互。

The service will handle GET requests to /greeting, optionally with a name parameter in the query string. The GET request should return a 200 OK response with JSON in the body to represent a greeting. It should resemble the following listing:

该服务将处理 `/greeting` 的 GET 请求，也可以在查询字符串中使用 name 参数。GET 请求应该返回一个包含 JSON 的 `200 OK` 响应来表示问候语。它应该与如下所示类似：

```json
{
  "id": 1,
  "content": "Hello, World!"
}
```

The id field is a unique identifier for the greeting, and content is the textual representation of the greeting.

id 字段是问候语的唯一标识符，而内容是问候语的文本表示。

To model the greeting representation, create a resource representation class. Provide a plain old Java object with fields, constructors, and accessors for the id and content data, as the following listing (from `src/main/java/com/example/restservicecors/Greeting.java`) shows:

要对问候语表示进行建模，请创建一个资源表示类。为 id 和内容数据提供一个普通的旧 Java 对象，包含字段、构造函数和访问器，如下所示（来自 `src/main/ Java /com/example/restservicecors/Greeting.java`）：

```java
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

Spring 使用 Jackson JSON 库自动将 Greeting 类型的实例封装到 JSON 中。

## Create a Resource Controller

**创建资源控制器**

In Spring’s approach to building RESTful web services, HTTP requests are handled by a controller. These components are easily identified by the @Controller annotation, and the GreetingController shown in the following listing (from `src/main/java/com/example/restservicecors/GreetingController.java`) handles GET requests for /greeting by returning a new instance of the Greeting class:

在 Spring 构建 RESTful web 服务的方式中，HTTP 请求由控制器处理。这些组件很容易通过 `@Controller` 注解定义，下面清单中显示的 GreetingController（来自 `src/main/java/com/example/restservicecors/GreetingController.java`）通过返回 Greeting 类的一个新实例来处理 GET 请求 `/greeting`：

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

这个控制器简洁而简单，但在引擎盖下有很多东西。我们一步一步地分析它。

The @RequestMapping annotation ensures that HTTP requests to /greeting are mapped to the greeting() method.

`@RequestMapping` 注解确保对 `/greeting` 的 HTTP 请求映射到 `greeting()` 方法。

> The preceding example uses the @GetMapping annotation, which acts as a shortcut for `@RequestMapping(method = RequestMethod.GET)`.

注意：前面的示例使用的 `@GetMapping` 注解是 `@RequestMapping(method = RequestMethod.GET)` 的简写。

`@RequestParam` binds the value of the name query string parameter into the name parameter of the greeting() method. This query string parameter is not required. If it is absent in the request, the defaultValue of World is used.

`@RequestParam` 将包含 name 参数的查询字符串值绑定到 `greeting()` 方法的名称参数中。此查询字符串参数不是必需的。如果它在请求中不存在，则使用「World」默认值。

The implementation of the method body creates and returns a new Greeting object, with the value of the id attribute based on the next value from the counter and the value of the content based on the query parameter or the default value. It also formats the given name by using the greeting template.

方法实现创建并返回一个新的 Greeting 对象，其中 id 属性的值基于计数器的下一个值，内容的值基于查询参数或默认值。它还通过使用问候语模板来格式化给定的 name。

A key difference between a traditional MVC controller and the RESTful web service controller shown earlier is the way that the HTTP response body is created. Rather than relying on a view technology to perform server-side rendering of the greeting data to HTML, this RESTful web service controller populates and returns a Greeting object. The object data is written directly to the HTTP response as JSON.

传统 MVC 控制器和 RESTful web 服务控制器之间的一个关键区别是 HTTP 响应体的创建方式。与依赖视图技术将问候语数据执行服务器端呈现为 HTML 不同，这个 RESTful web 服务控制器填充并返回一个 Greeting 对象。对象数据直接以 JSON 的形式写入 HTTP 响应。

To accomplish this, the `@ResponseBody` annotation on the greeting() method tells Spring MVC that it does not need to render the greeting object through a server-side view layer. Instead, the returned greeting object is the response body and should be written out directly.

为此，greeting() 方法上的 `@ResponseBody` 注解告诉 Spring MVC，它不需要通过服务器端视图层呈现 greeting 对象。相反，返回的 greeting 对象是响应体，应该直接写出来。

The Greeting object must be converted to JSON. Thanks to Spring’s HTTP message converter support, you need not do this conversion manually. Because Jackson is on the classpath, Spring’s MappingJackson2HttpMessageConverter is automatically chosen to convert the Greeting instance to JSON.

Greeting 对象必须转换为 JSON。由于 Spring 的 HTTP 消息转换器支持，不需要手动执行此转换。因为 J ackson 位于类路径中，所以会自动调用 Spring 的 MappingJackson2HttpMessageConverter 将 Greeting 实例转换为 JSON。

## Enabling CORS

**启用跨域资源共享（CORS）**

You can enable cross-origin resource sharing (CORS) from either in individual controllers or globally. The following topics describe how to do so:

你可以在单个控制器或全局控制器中启用跨源资源共享（CORS）。以下主题描述了如何做到这一点：

- Controller Method CORS Configuration

控制器方法的 CORS 配置

- Global CORS configuration

全局 CORS 配置

## Controller Method CORS Configuration

**控制器方法的 CORS 配置**

So that the RESTful web service will include CORS access control headers in its response, you have to add a @CrossOrigin annotation to the handler method, as the following listing (from `src/main/java/com/example/restservicecors/GreetingController.java`) shows:

为了使 RESTful web 服务在其响应中包含 CORS 访问控制头，必须向处理程序方法添加一个 `@CrossOrigin` 注释，如下所示（来自 `src/main/java/com/example/restservicecors/GreetingController.java`）：

```java
	@CrossOrigin(origins = "http://localhost:9000")
	@GetMapping("/greeting")
	public Greeting greeting(@RequestParam(required=false, defaultValue="World") String name) {
		System.out.println("==== in greeting ====");
		return new Greeting(counter.incrementAndGet(), String.format(template, name));
	}
```

This @CrossOrigin annotation enables cross-origin resource sharing only for this specific method. By default, its allows all origins, all headers, and the HTTP methods specified in the @RequestMapping annotation. Also, a maxAge of 30 minutes is used. You can customize this behavior by specifying the value of one of the following annotation attributes:

`@CrossOrigin` 注解只支持这个特定方法的跨源资源共享。默认情况下，它允许所有的源、所有的头和在 @RequestMapping 注解中指定的 HTTP 方法。此外，最大使用 30 分钟。可以通过指定下列注解属性之一的值来自定义此行为：

- origins
- methods
- allowedHeaders
- exposedHeaders
- allowCredentials
- maxAge.

In this example, we allow only http://localhost:9000 to send cross-origin requests.

在这里例子中，我们只允许 http://localhost:9000 发送跨域请求。

> You can also add the @CrossOrigin annotation at the controller class level as well, to enable CORS on all handler methods of this class.

你同样可以将 @CrossOrigin 注解应用在类级别，以便让其中所有方法开启 CORS

## Global CORS configuration

**全局 CORS 配置**

In addition (or as an alternative) to fine-grained annotation-based configuration, you can define some global CORS configuration as well. This is similar to using a Filter but can be declared within Spring MVC and combined with fine-grained @CrossOrigin configuration. By default, all origins and GET, HEAD, and POST methods are allowed.

此外(或作为基于细粒度注释的配置的替代方法)，还可以定义一些全局 CORS 配置。这类似于使用过滤器，但可以在 Spring MVC 中声明，并与细粒度的 `@CrossOrigin` 配置相结合。默认情况下，允许使用所有域、GET、HEAD 和 POST 方法。

The following listing (from `src/main/java/com/example/restservicecors/GreetingController.java)` shows the greetingWithJavaconfig method in the GreetingController class:

如下所示（源码参考 `src/main/java/com/example/restservicecors/GreetingController.java)`）：

```java
	@GetMapping("/greeting-javaconfig")
	public Greeting greetingWithJavaconfig(@RequestParam(required=false, defaultValue="World") String name) {
		System.out.println("==== in greeting ====");
		return new Greeting(counter.incrementAndGet(), String.format(template, name));
	}
```

> The difference between the greetingWithJavaconfig method and the greeting method (used in the controller-level CORS configuration) is the route (/greeting-javaconfig rather than /greeting) and the presence of the @CrossOrigin origin.

greetingWithJavaconfig 方法和 greeting 方法（在控制器级 CORS 配置中使用）之间的区别是路由(/greeting-javaconfig 而不是 /greeting)和 `@CrossOrigin` 来源的存在。

The following listing (from src/main/java/com/example/restservicecors/RestServiceCorsApplication.java) shows how to add CORS mapping in the application class:

下面的清单(源码路径 `src/main/java/com/example/restservicecors/RestServiceCorsApplication.java`）展示了如何在应用程序类中添加 CORS 映射：

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

您可以轻松地更改任何属性（例如本例中的 allowedOrigins），也可以将这种 CORS 配置应用于特定的路径模式。

> You can combine global- and controller-level CORS configuration.

你可以结合全局和控制器两种级别的 CORS 配置

## Creating the Application Class

**生成 Application 类**

The Spring Initializr creates a bare-bones application class for you. The following listing (from initial/src/main/java/com/example/restservicecors/RestServiceCorsApplication.java) shows that initial class:

Spring Initializr 创建了一个基本的应用程序类。下面的清单（来自 `initial/src/main/java/com/example/restservicecors/RestServiceCorsApplication.java`）显示了这个类：

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

你需要添加一个方法来配置如何处理跨源资源共享。下面的清单（来自 `complete/src/main/java/com/example/restservicecors/RestServiceCorsApplication.java`）展示了如何做到这一点：

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

完整的应用类如下所示：

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

`@SpringBootApplication` 是一个方便的注解，它添加了以下所有内容：

- `@Configuration`: Tags the class as a source of bean definitions for the application context.

将该类标记为应用程序上下文的 bean 定义源。

- `@EnableAutoConfiguration`: Tells Spring Boot to start adding beans based on classpath settings, other beans, and various property settings. For example, if spring-webmvc is on the classpath, this annotation flags the application as a web application and activates key behaviors, such as setting up a DispatcherServlet.

告诉 Spring Boot 根据类路径设置、其他 bean 和各种属性设置开始添加 bean。例如，如果 spring-webmvc 位于 classpath，则该注解将应用程序标记为 web 应用程序并激活关键行为，例如设置 DispatcherServlet。

- `@ComponentScan`: Tells Spring to look for other components, configurations, and services in the com/example package, letting it find the controllers.

告诉 Spring 在 com/example 包中查找其他组件、配置和服务，让它查找控制器。

The main() method uses Spring Boot’s SpringApplication.run() method to launch an application. Did you notice that there was not a single line of XML? There is no web.xml file, either. This web application is 100% pure Java and you did not have to deal with configuring any plumbing or infrastructure.

main() 方法使用 Spring Boot 的 SpringApplication.run() 方法来启动应用程序。您注意到没有一行 XML 吗？也没有 web.xml 文件。这个 web 应用程序是 100% 纯 Java 的，不需要配置任何管道或基础设施。

**译注：以下部分过于简单，且不涉及 CORS，不译。**

## Build an executable JAR

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
$(document).ready(function () {
  $.ajax({
    url: "http://localhost:8080/greeting",
  }).then(function (data, status, jqxhr) {
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

（原文有图，略）

If the service response includes the CORS headers, then the ID and content are rendered into the page. But if the CORS headers are missing (or insufficiently defined for the client), the browser fails the request and the values are not rendered into the DOM. In that case, you should see the following:

（原文有图，略）

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
