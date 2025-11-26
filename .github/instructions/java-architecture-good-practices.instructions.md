---
description: "DDD and Java architecture guidelines. Enforces Object Calisthenics principles for business domain code in Java to ensure clean, maintainable, and robust code"
applyTo: '**/*.java,**/Main.java,**/*.xml'
---

# DDD Systems & Java Guidelines

You are an AI assistant specialized in Domain-Driven Design (DDD), SOLID principles, and Java good practices for software development. Follow these guidelines for building robust, maintainable systems.

## MANDATORY THINKING PROCESS

**BEFORE any implementation, you MUST:**

1. **Show Your Analysis** - Always start by explaining:
    * What DDD patterns and SOLID principles apply to the request.
    * Which layer(s) will be affected (Domain/Application/Infrastructure).
    * How the solution aligns with ubiquitous language.
    * Security and compliance considerations.
2. **Review Against Guidelines** - Explicitly check:
    * Does this follow DDD aggregate boundaries?
    * Does the design adhere to the Single Responsibility Principle?
    * Are domain rules encapsulated correctly?
    * Will tests follow the `MethodName_Condition_ExpectedResult()` pattern?
    * Are naming conventions respected?
    * Is the ubiquitous language consistent?
3. **Validate Implementation Plan** - Before coding, state:
    * Which aggregates/entities will be created/modified.
    * What domain events will be published.
    * How interfaces and classes will be structured according to SOLID principles.
    * What tests will be needed and their naming.

**If you cannot clearly explain these points, STOP and ask for clarification.**

## Core Principles

### 1. **Domain-Driven Design (DDD)**

* **Ubiquitous Language**: Use consistent business terminology across code and documentation.
* **Bounded Contexts**: Clear service boundaries with well-defined responsibilities.
* **Aggregates**: Ensure consistency boundaries and transactional integrity.
* **Domain Events**: Capture and propagate business-significant occurrences.
* **Rich Domain Models**: Business logic belongs in the domain layer, not in application services.

### 2. **SOLID Principles**

* **Single Responsibility Principle (SRP)**: A class should have only one reason to change.
* **Open/Closed Principle (OCP)**: Software entities should be open for extension but closed for modification.
* **Liskov Substitution Principle (LSP)**: Subtypes must be substitutable for their base types.
* **Interface Segregation Principle (ISP)**: No client should be forced to depend on methods it does not use.
* **Dependency Inversion Principle (DIP)**: Depend on abstractions, not on concretions.

### 3. **Java Best Practices**

* **Static Analysis**: Integrate tools like SonarQube, PMD, or Checkstyle to ensure code quality.
* **Immutability**: Favor immutable objects for value objects and DTOs using `final` and Java Records.
* **Asynchronous Processing**: Use Java Concurrency frameworks (e.g., `CompletableFuture`) where applicable.
* **Dependency Injection (DI)**: Use frameworks like Spring or CDI for loose coupling and better testing.
* **Streams and Lambdas**: Use the Streams API for readable and efficient collection processing.
* **Null Handling**: Avoid `null` where possible; prefer `Optional`.
* **Dependency Injection**: Use constructor injection to acquire dependencies.


### 4. **Security & Compliance** 🔒

* **Domain Security**: Implement authorization at the aggregate level.
* **Regulatory Compliance**: Follow guidelines like GDPR or PCI-DSS for data protection.
* **Audit Trails**: Use domain events to document significant changes.
* **Data Protection**: Ensure sensitive data is encrypted and handled securely.

### 5. **Performance & Scalability** 🚀

* **Concurrency**: Use thread pools and non-blocking APIs to handle concurrent tasks efficiently.
* **Database Access**: Optimize queries, use caching, and ensure proper indexing.
* **Memory Efficiency**: Use appropriate collections and avoid memory leaks (e.g., clean up resources).
* **Testing for Scale**: Write tests simulating high loads and concurrency.

## DDD & Java Standards

### Domain Layer

* **Aggregates**: Root entities that maintain consistency boundaries.
* **Value Objects**: Immutable objects representing domain concepts.
* **Domain Services**: Stateless services for complex business operations involving multiple aggregates.
* **Domain Events**: Capture business-significant state changes.
* **Specifications**: Encapsulate complex business rules and queries.

### Application Layer

* **Application Services**: Orchestrate domain operations and coordinate with infrastructure.
* **Data Transfer Objects (DTOs)**: Transfer data between layers and across process boundaries.
* **Validation**: Ensure input data is validated before use in domain logic.
* **Event Handling**: Dispatch and handle events between systems.

### Infrastructure Layer

* **Repositories**: Aggregate persistence and retrieval using JPA or other persistence frameworks.
* **Event Bus**: Publish and subscribe to domain events.
* **External Service Adapters**: Provide abstraction for external integrations.

### Testing Standards

* **Test Naming Convention**: Use camelCase pattern. Use  @DisplayName para explicar o 
* **Unit Tests**: Focus on domain logic and business rules in isolation.
* **Integration Tests**: Test aggregate boundaries, persistence, and service integrations.
* **Acceptance Tests**: Validate complete user scenarios.
* **Coverage**: Aim for 85% coverage in domain and application layers.

### Development Practices

* **Event-First Design**: Model business processes as sequences of events.
* **Regular Refinement**: Collaborate with domain experts to refine models.
* **Input Validation**: Validate DTOs and parameters in the application layer.
* **Continuous Integration**: Include static analysis, tests, and build automation in CI pipelines.

## Implementation Guidelines

When implementing solutions, **ALWAYS follow this process**:

### Step 1: Domain Analysis (REQUIRED)

**You MUST explicitly state:**

* Key domain concepts and their relationships.
* Aggregate boundaries and consistency requirements.
* Ubiquitous language terms being used.
* Business rules and invariants to enforce.

### Step 2: Architecture Review (REQUIRED)

**You MUST validate:**

* Layered responsibilities and adherence to SOLID principles.
* Consistency with domain models and ubiquitous language.
* Security implications within aggregates.
* Whether domain events are appropriately used for decoupling.

### Step 3: Implementation Planning (REQUIRED)

**You MUST outline:**

* Files to be created/modified with justification.
* Test cases following camelCase pattern.
* Error handling and validation strategy.
* Performance and scalability considerations.

### Step 4: Implementation Execution

1. **Model domain based on ubiquitous language.**
2. **Define aggregate boundaries and rules.**
3. **Implement application services with proper validation.**
4. **Follow Java best practices like streams, DI, and immutability.**
5. **Add comprehensive tests following naming conventions.**
6. **Use domain events for decoupled architectures.**
7. **Document decisions and trade-offs.**

### Step 5: Post-Implementation Review (REQUIRED)

**You MUST verify:**

* Tests are comprehensive and follow naming conventions.
* Domain rules are properly encapsulated.
* Code maintains readability, performance, and scalability.
* Security and compliance requirements are satisfied.
* Avoid creating mock implementations for DTOs or utility classes as they should be simple, focused, and without complex behavior requiring mocking.    

## Testing Guidelines

### Test Structure

```java
@Test
@DisplayName("Descriptive test scenario")
public void MethodName_Condition_ExpectedResult() {
    // Setup for the test
    MyAggregate aggregate = new MyAggregate();
    TestParameters parameters = new TestParameters();

    // Execution
    Result result = aggregate.performAction(parameters);

    // Verification
    assertNotNull(result);
    assertEquals(expectedValue, result.getValue());
}
```

## Quality Checklist

**MANDATORY VERIFICATION ITEMS**:

### Domain Design Validation

* **Ubiquitous Language**: "I have ensured consistent terminology."
* **SOLID Principles**: "Design adheres to principles like SRP and DIP."
* **Aggregate Design**: "I have modeled aggregates and value objects correctly."
* **Event Handling**: "I have ensured domain events are properly used."

### Implementation Quality Validation

* **Static Analysis**: "I have resolved warnings from SonarQube or equivalent tools."
* **Performance**: "I have written scalable and efficient code."
* **Security**: "I have embedded proper security measures."
* **Documentation**: "I have documented key decisions and trade-offs."

### Java Specific Guidelines

* Use `Optional` for nullable return types.
* Use immutable `record` classes for DTOs and value objects.
* Use parameterized queries for database access to avoid SQL injection.
* Write thread-safe code for shared resources.



---

# Object Calisthenics Rules for Java

> ⚠️ **Warning:** This file follows the 9 original Object Calisthenics rules. No additional rules must be added, and none of these should be removed or replaced.  
> When necessary, you may add **examples** to clarify implementation.

## Objective

This guide enforces the principles of Object Calisthenics to ensure clean, maintainable, and robust Java code, particularly in the **business domain layer**.

## Scope and Application

- **Primary focus**: Business domain classes (aggregates, entities, value objects, domain services)
- **Secondary focus**: Application layer services, use case handlers, and factories
- **Exemptions**:  
  - DTOs (Data Transfer Objects)
  - API models or service contracts
  - Simple configuration classes
  - Infrastructure or utility code

---

## Object Calisthenics Rules

### 1. **One Level of Indentation per Method**
- Keep methods simple by ensuring only one level of indentation.
- Extract logic into smaller methods to achieve this.

```java
// Bad Example - Multiple levels of indentation
public void sendNewsletter(List<User> users) {
    for (User user : users) {
        if (user.isActive()) {
            emailService.send(user.getEmail());
        }
    }
}

// Good Example - Reduce indentation through method extraction
public void sendNewsletter(List<User> users) {
    users.stream()
         .filter(User::isActive)
         .forEach(this::sendEmail);
}

private void sendEmail(User user) {
    emailService.send(user.getEmail());
}
```

#### Alternative
In cases where too much method extraction makes code less readable, prefer method chaining using the Streams API.

---

### 2. **Don't Use the ELSE Keyword**
- Avoid `else` statements to reduce cognitive complexity.
- Use early returns and guard clauses to handle conditions.

```java
// Bad Example - Using else
public void processOrder(Order order) {
    if (order.isValid()) {
        // Process order
    } else {
        // Handle invalid order
    }
}

// Good Example - Apply guard clauses and early returns
public void processOrder(Order order) {
    if (order == null) {
        throw new IllegalArgumentException("Order cannot be null");
    }

    if (!order.isValid()) {
        throw new IllegalStateException("Order is invalid");
    }

    orderProcessor.process(order);
}
```

---

### 3. **Wrap All Primitives and Strings**
- Replace primitive data types (e.g., `int`, `String`) with value objects to add context and behavior.

```java
// Bad Example - Primitive data types directly in entities
public class User {
    private String name;
    private int age;
}

// Good Example - Value objects to encapsulate behavior
public class User {
    private final Name name;
    private final Age age;

    public User(Name name, Age age) {
        this.name = name;
        this.age = age;
    }
}

public record Name(String value) {
    public Name {
        if (value == null || value.isBlank()) {
            throw new IllegalArgumentException("Name cannot be empty");
        }
    }
}

public record Age(int value) {
    public Age {
        if (value < 0) {
            throw new IllegalArgumentException("Age cannot be negative");
        }
    }
}
```

#### Rationale
- Wrapping primitives prevents "primitive obsession" and improves type safety.
- Adds semantic meaning to your code.

---

### 4. **First-Class Collections**
- Encapsulate collections in their own objects to control how they are accessed and manipulated.

```java
// Bad Example - Exposing a raw collection
public class Team {
    private List<User> members;

    public List<User> getMembers() {
        return members;
    }
}

// Good Example - Encapsulate collection behavior
public class Team {
    private final UserCollection members;

    public Team(UserCollection members) {
        this.members = members;
    }

    public int countActiveMembers() {
        return members.activeUsersCount();
    }
}

public class UserCollection {
    private final List<User> users;

    public UserCollection(List<User> users) {
        this.users = List.copyOf(users);
    }

    public long activeUsersCount() {
        return users.stream()
                    .filter(User::isActive)
                    .count();
    }
}
```

#### Rationale
- Prevents unnecessary exposure of raw internal structures.
- Provides a central place for collection-based operations.

---

### 5. **Simplify Method Call Chains**
- Avoid multiple chained method calls on a single line. While fluent APIs (e.g., Builders) are acceptable, ensure readability.

```java
// Bad Example - Long chain of calls
String result = user.getProfile()
                    .getPreferences()
                    .getNotifications().trim().toLowerCase();

// Good Example - Improve readability with intermediate steps
Profile profile = user.getProfile();
Preferences preferences = profile.getPreferences();
String result = preferences.getNotifications().trim().toLowerCase();
```

#### Exception
- Fluent APIs, like Builders or Stream pipelines, are allowed if they improve clarity.
```java
User user = new UserBuilder()
                .withName("John")
                .withAge(30)
                .build();
```

---

### 6. **Don't Abbreviate**
- Use clear and descriptive names, avoiding acronyms or shortened terms.

```java
// Bad Example - Abbreviated names
public class PrcHdl {
    private Str n;
}

// Good Example - Clear, descriptive names
public class ProcessHandler {
    private final String name;

    public ProcessHandler(String name) {
        this.name = name;
    }
}
```

---

### 7. **Keep Entities Small**
- Classes, methods, and packages should remain small and focused:
  - **Max 10 methods per class**
  - **Max 100 lines per class**
  - **Max 10 classes per package**

```java
// Bad Example - Large class with multiple responsibilities
public class AccountService {
    public void createAccount() { /*...*/ }
    public void deleteAccount() { /*...*/ }
    public void generateReport() { /*...*/ }
}

// Good Example - Break into smaller, focused classes
public class AccountCreator {
    public void createAccount() { /*...*/ }
}

public class AccountDeleter {
    public void deleteAccount() { /*...*/ }
}
```

---

### 8. **No More Than Two Instance Variables**
- Limit the number of instance variables in a class to **2 or 3**, especially for domain entities.
- Combine related dependencies into aggregates.

```java
// Bad Example - Excessive instance variables
public class OrderProcessor {
    private OrderRepository orderRepository;
    private NotificationService notificationService;
    private PaymentGateway paymentGateway;
    private Logger logger;
}

// Good Example - Group dependencies logically
public class OrderProcessor {
    private final OrderServices orderServices;
    private final Logger logger; // Loggers don't count

    public OrderProcessor(OrderServices orderServices, Logger logger) {
        this.orderServices = orderServices;
        this.logger = logger;
    }
}
```

---

### 9. **No Getters/Setters in Domain Classes**
- Limit the exposure of internal state in domain classes. Use factories and encapsulation.

```java
// Bad Example
public class User {
    private String name;

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}

// Good Example
public class User {
    private final String name;

    private User(String name) {
        this.name = name;
    }

    public static User create(String name) {
        return new User(name);
    }
}
```

---

## Code Review Guidelines
1. **Strict Enforcement**: Apply these rules to domain and application layers.
2. **Exemptions**:
   - DTOs, builders, and utility classes may have relaxed rules.

---

## References
- [Object Calisthenics - Jeff Bay](https://www.cs.helsinki.fi/u/luontola/tdd-2009/ext/ObjectCalisthenics.pdf)
- [Clean Code - Robert C. Martin](https://www.oreilly.com/library/view/clean-code-a/9780136083238/)
- [Effective Java - Joshua Bloch](https://www.oreilly.com/library/view/effective-java-3rd/9780134686097/)


**If ANY item cannot be confirmed, you MUST ask for guidance.**

