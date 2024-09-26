# Spring Annotations Overview

## Stereotype Annotations

These annotations indicate the role or layer of a class within the application.

### `@Component`
`@Component` is a generic stereotype for any Spring-managed component. When I mark a class with this annotation, I'm telling Spring to create an instance of this class and manage it, making it available for dependency injection throughout the application.

### `@Controller`
This is a `@Component` used for web MVC controllers. In a web application, classes annotated with `@Controller` handle incoming web requests and decide what response to send back to the user, such as rendering a webpage.

### `@Service`
`@Service` is a `@Component` for service layer components. Classes marked with this annotation contain the business logic of the application—the core operations and calculations that are central to its functionality.

### `@Repository`
This annotation is a `@Component` for data access layer components. When I have a class that interacts with the database (fetching or saving data), I use `@Repository` to indicate its role. This also enables Spring to translate database exceptions into a consistent, more manageable form.

## Web and RESTful Services Annotations

### `@RestController`
`@RestController` is a `@Controller` for RESTful web services. Classes annotated with this handle web requests and return data directly (like JSON), rather than rendering a view. This is useful for building APIs that clients can consume.

### `@RequestMapping`
Used to map HTTP requests to handler methods in controllers. By applying this annotation at the class or method level, I can define the URLs and HTTP methods that the controller methods will handle.

### `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, `@PatchMapping`
Specialized versions of `@RequestMapping` for specific HTTP methods.

### `@RequestBody`
`@RequestBody` is an annotation used to bind the HTTP request body to a Java object. When a client sends data to the server, such as JSON or XML, @RequestBody allows me to directly map this data to a method parameter in a controller.

## Configuration Annotations

Annotations used to define and manage the configuration and beans within the Spring application.

### `@Configuration`
This is a specialized `@Component` for configuration classes. By marking a class with `@Configuration`, I'm telling Spring that this class contains configuration and setup instructions, such as bean definitions and other settings that the application needs to run properly.

### `@Bean`
Used to define a bean in a Spring application. A bean is an object that is managed by the Spring container (IOC). By using `@Bean`, I can specify how to create an instance of an object that Spring will manage and supply wherever it's needed in the application.

### `@Autowired`
Used for automatic dependency injection. When I mark a constructor, method, or field with `@Autowired`, Spring will automatically provide the required dependencies. This helps reduce boilerplate code by eliminating the need to manually instantiate dependent objects. Spring scans for `@Compontents` matching the type of the dependency and injects them.

If there are multiple beans of the same type, I can use `@Qualifier` to specify which bean to inject.

Mark with `@Primary` to indicate the primary bean when there are multiple beans of the same type.


### `@Qualifier`
Used to specify which bean to inject when there are multiple beans of the same type. By applying `@Qualifier` to a field or parameter, I can indicate the name of the bean that should be injected. The name should match the value of the `@Bean` annotation that defines the bean but the first letter should be lowercase.

## JPA

### `@Entity`
`@Entity` is used to mark a class as a JPA entity, which means it will be mapped to a database table. By annotating a class with `@Entity`, I'm telling JPA to treat this class as a persistent entity, allowing me to save, update, and query instances of this class in the database.

### `@Table`
`@Table` is used to specify the details of the database table that an entity will be mapped to. By applying this annotation, I can define the table name, schema, and other properties of the database table that the entity represents.

## Other

### `@Transactional`
By applying this annotation, I can ensure that a sequence of operations is executed within a database transaction, providing consistency and rollback capabilities.

### `@EnableWebSecurity`
Is an annotation that enables Spring Security in a Spring Boot application. By applying this annotation to a configuration class, I can customize the security configuration through method overrides. Allowing me to define security rules such as authentication mechanisms, URL restrictions, and filter chains.

### `@Value`
`@Value` is used to inject values from properties files or environment variables into Spring beans. By using this annotation, I can externalize configuration properties and access them in my application code.

### `@SpringBootApplication`
Composed of the following annotations: `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`. This annotation is used to mark the main class of a Spring Boot application, indicating that this class is the entry point for the application and that it should be scanned for components and auto-configured by Spring Boot.

