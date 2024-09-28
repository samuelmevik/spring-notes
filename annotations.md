# Spring Annotations Overview

## Core Annotations

### `@Component`
`@Component` is a generic stereotype for any Spring-managed component. When I mark a class with this annotation, I'm telling Spring to create an instance of this class and manage it, making it available for dependency injection throughout the application.

### `@Controller`
This is a `@Component` used for web MVC controllers. In a web application, classes annotated with `@Controller` handle incoming web requests and decide what response to send back to the user, such as rendering a webpage.

### `@Service`
`@Service` is a `@Component` for service layer components. Classes marked with this annotation contain the business logic of the application—the core operations and calculations that are central to its functionality.

### `@ExceptionHandler`
`@ExceptionHandler` is used to define methods that handle exceptions thrown by request mapping methods. By applying this annotation to a method in a `@Controller` or `@RestController`, I can specify how to handle exceptions that occur during the execution of a request mapping method.

```java
    @ExceptionHandler
    public ResponseEntity<StudentErrorResponse> handleStudentNotFoundException(StudentNotFoundException e) {
        var status = HttpStatus.NOT_FOUND;
        StudentErrorResponse response = StudentErrorResponse.builder()
                .status(status.value())
                .message(e.getMessage())
                .timeStamp(System.currentTimeMillis())
                .build();
        return new ResponseEntity<>(response, status);
    }
```

### `@ControllerAdvice`
`@ControllerAdvice` is an annotation used to define global exception handlers that apply to all `@Controller` classes. By applying this annotation to a class, I can define methods that handle exceptions thrown by any `@RequestMapping` method in any `@Controller` class. For Restful services, I can use `@RestControllerAdvice`.


## Web and RESTful Services Annotations

### `@RestController`
`@RestController` is a `@Controller` for RESTful web services. Classes annotated with this handle web requests and return data directly (like JSON), rather than rendering a view. This is useful for building APIs that clients can consume.

### `@RestControllerAdvice`
`@RestControllerAdvice` is a specialized version of `@ControllerAdvice` that applies to `@RestController` classes. By applying this annotation to a class, I can define global exception handlers that apply to all `@RestController` classes, allowing me to handle exceptions thrown by any RESTful web service method.

### `@RequestMapping`
Used to map HTTP requests to handler methods in controllers. By applying this annotation at the class or method level, I can define the URLs and HTTP methods that the controller methods will handle.

### `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, `@PatchMapping`
Specialized versions of `@RequestMapping` for specific HTTP methods.

### `@RequestBody`
`@RequestBody` is an annotation used to bind the HTTP request body to a Java object. When a client sends data to the server, such as JSON or XML, @RequestBody allows me to directly map this data to a method parameter in a controller.

### `@PathVariable`
`@PathVariable` is used to extract values from the URI path and map them to method parameters in a controller. By applying this annotation to a method parameter, I can specify which part of the URI path should be bound to that parameter.

Example:
```java

@DeleteMapping(path = "{id}")
public void deleteEmployee(@PathVariable Long id) {
    employeeService.deleteEmployee(id);
}

```       

### `@RequestParam`
`@RequestParam` is used to extract query parameters from the URL and map them to method parameters in a controller. By applying this annotation to a method parameter, I can specify which query parameter should be bound to that parameter.

Example:
```java
@GetMapping
public List<Employee> getEmployees(@RequestParam String department) {
    return employeeService.getEmployeesByDepartment(department);
}
```


## Configuration Annotations

Annotations used to define and manage the configuration and beans within the Spring application.

### `@Configuration`
This is a specialized `@Component` for configuration classes. By marking a class with `@Configuration`, I'm telling Spring that this class contains configuration and setup instructions, such as bean definitions and other settings that the application needs to run properly.

### `@Bean`
Used to define a bean in a Spring application. A bean is an object that is managed by the Spring container (IOC). By using `@Bean`, I can specify how to create an instance of an object that Spring will manage and supply wherever it's needed in the application. Good to use when Spring needs to instantiate a class that I don't have control over, such as a third-party library. `@Bean` can have a name attribute to specify the name of the bean.

### `@Autowired`
Used for automatic dependency injection. When I mark a constructor, method, or field with `@Autowired`, Spring will automatically provide the required dependencies. This helps reduce boilerplate code by eliminating the need to manually instantiate dependent objects. Spring scans for `@Compontents` matching the type of the dependency and injects them.

If there are multiple beans of the same type, I can use `@Qualifier` to specify which bean to inject.

Mark with `@Primary` to indicate the primary bean when there are multiple beans of the same type. `@Qualifier` overrides `@Primary`.

### `@Lazy`
`@Lazy` is used to indicate that a bean should be lazily initialized. By applying this annotation to a bean definition, I can tell Spring to only create an instance of the bean when it's first requested, rather than creating it eagerly at startup.

spring.main.lazy-initialization=true can be set in the application.properties file to enable lazy initialization for all beans. Makes sense when developing large applications where not all beans are needed at startup.


### `@Qualifier`
Used to specify which bean to inject when there are multiple beans of the same type. By applying `@Qualifier` to a field or parameter, I can indicate the name of the bean that should be injected. The name should match the value of the `@Bean` annotation that defines the bean but the first letter should be lowercase.

## JPA

### `@Repository`
This annotation is a `@Component` for data access layer components. When I have a class that interacts with the database (fetching or saving data), I use `@Repository` to indicate its role. This also enables Spring to translate database exceptions into a consistent, more manageable form.

### `@Id`
`@Id` is used to mark a field as the primary key of an entity. I can specify the strategy for generating primary key values by using `@GeneratedValue` in conjunction with `@Id`.

It is possible to use a custom primary key generator by implementing the `IdentifierGenerator` interface and specifying the generator class in the `@GeneratedValue` annotation.

### `@Entity`
`@Entity` is used to mark a class as a JPA entity, which means it will be mapped to a database table. By annotating a class with `@Entity`, I'm telling JPA to treat this class as a persistent entity, allowing me to save, update, and query instances of this class in the database.

### `@Table`
`@Table` is used to specify the details of the database table that an entity will be mapped to. By applying this annotation, I can define the table name, schema, and other properties of the database table that the entity represents. If the table name is the same as the entity name, meaning that if the is a refactoring of the entity name, the table name will be updated as well, breaking things.

### `@Column`
`@Column` is used to specify the details of a column in a database table that an entity field will be mapped to. By applying this annotation to a field in an entity class, I can define the column name, type, length, and other properties of the database column that the field represents. If the column name is the same as the field name, meaning that if the is a refactoring of the field name, the column name will be updated as well, breaking things.

### `@Transactional`
By applying this annotation, I can ensure that a sequence of operations is executed within a database transaction, providing consistency and rollback capabilities. When I mark a method with `@Transactional`, Spring will manage the transaction boundaries, committing the transaction if the method completes successfully and rolling it back if an exception occurs. As soon as i want to perform a database post or put operation, I should use `@Transactional`.

### `@OneToMany`, `@ManyToOne`, `@OneToOne`, `@ManyToMany`
These annotations are used to define relationships between entities in a JPA application. By applying these annotations to fields in entity classes, I can specify how entities are related to each other.

#### `Uni-directional` vs `Bi-directional` relationships
In a uni-directional relationship, one entity has a reference to another entity, but the other entity does not have a reference back. In a bi-directional relationship, both entities have references to each other, allowing navigation in both directions.

#### `CascadeType`
Defines how operations on one entity should affect related entities. By default, no operations are cascaded.

If I want to specify precisely I can use the following:

```java
@OneToOne(cascade = {CascadeType.PERSIST, CascadeType.MERGE, ...})
```

- `Persist`: When a new entity is persisted, related entities are also persisted.
- `Merge`: When an entity is merged, related entities are also merged.
- `Remove`: When an entity is removed, related entities are also removed.
- `Refresh`: When an entity is refreshed, related entities are also refreshed.
- `Detach`: When an entity is detached (not associated w/ session), related entities are also detached.
- `All`: All of the above cascade types.

## Other

### `@EnableWebSecurity`
Is an annotation that enables Spring Security in a Spring Boot application. By applying this annotation to a configuration class, I can customize the security configuration through method overrides. Allowing me to define security rules such as authentication mechanisms, URL restrictions, and filter chains.

### `@Value`
`@Value` is used to inject values from properties files or environment variables into Spring beans. By using this annotation, I can externalize configuration properties and access them in my application code.

### `@SpringBootApplication`
Composed of the following annotations: `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`. This annotation is used to mark the main class of a Spring Boot application, indicating that this class is the entry point for the application and that it should be scanned for components and auto-configured by Spring Boot.

### `@Scope`
I can use this annotation to specify the scope of a bean. The scope determines the lifecycle and visibility of a bean instance. Common scopes include `singleton`, `prototype`, `request`, `session`, and `application`. The default scope is `singleton`.
