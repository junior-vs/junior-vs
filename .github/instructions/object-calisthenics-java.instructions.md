---
applyTo: '**/*.java'
description: Enforces Object Calisthenics principles for business domain code in Java to ensure clean, maintainable, and robust code
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
  - **Max 50 lines per class**
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