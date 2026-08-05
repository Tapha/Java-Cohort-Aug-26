# 🧠 Exercise: From Memory to SOLID ☕️⚙️

## Goal

By the end of this exercise, you should be able to explain how a Java program moves from:

```text
memory
→ objects
→ dependencies
→ design problems
→ SOLID solutions
```

This is not just about writing code.

It is about learning how software systems are shaped.

---

# Part 1 — Memory Foundation 🧱

Look at this Java code:

```java
public class Main {

    public static void main(String[] args) {
        User user = new User("amina@example.com", "Amina");
        System.out.println(user.getEmail());
    }
}

class User {
    private String email;
    private String name;

    public User(String email, String name) {
        this.email = email;
        this.name = name;
    }

    public String getEmail() {
        return email;
    }
}
```

## Questions

1. Which parts of this code are likely stored on the **stack**?
   1. the variable user, the arguments(args) and the main method?
2. Which object is created on the **heap**?
   1. User user
3. What does the variable `user` refer to?
   1. it refers to the User object that was made using new User()
4. When `main()` finishes, what can eventually happen to the `User` object?
   1. garbage collection should delete it
5. In your own words, explain this line:
    1. when you create an object from a class, since is is now a real thing, and it keeps its own data together, so its like an organises block of memory
```text
An object is structured memory.
```

    

---

# Part 2 — Objects and Responsibilities 🧩

Now look at this class:

```java
public class UserService {

    public void registerUser(String email, String name) {
        validateEmail(email);
        saveUser(email, name);
        sendWelcomeEmail(email);
        generateUserReport(email);
    }

    private void validateEmail(String email) {
        System.out.println("Validating email: " + email);
    }

    private void saveUser(String email, String name) {
        System.out.println("Saving user to database");
    }

    private void sendWelcomeEmail(String email) {
        System.out.println("Sending welcome email");
    }

    private void generateUserReport(String email) {
        System.out.println("Generating user report");
    }
}
```

## Questions

1. What is this class responsible for?
   1. for creating a new user, and all the introductory processess associated with that
2. How many different reasons could this class change?
   1. alot, every method could change depending on the context / requirements
3. which method relates to validation?
   1. validateEmail
4. Which method relates to persistence/database work?
   1. SaveUser
5. Which method relates to email?
   1. sendWelcomeEmail
6. Which method relates to reporting?
   1. generateUserReport
7. Why could this class become difficult to maintain as the system grows?
   1. becuase it does many things at once, as the program gets bigger, the class is will become a bottleneck to do all these things efficiently

---

# Part 3 — Apply SRP 🎯

The Single Responsibility Principle says:

```text
A class should have one main reason to change.
```

Refactor the responsibilities from `UserService` into smaller classes.

You do not need to write full method bodies yet.

Just create the class names and method names.

## Starter Structure

```java
public class UserRegistrationService {

    private EmailValidator validator = new EmailValidator();
    private UserRepository repository = new UserRepository();
    private WelcomeEmailSender emailSender = new WelcomeEmailSender();
    private UserReportService reportService = new UserReportService();

    public void registerUser(String email, String name) {
        if (validator.validate(email)) {
            repository.saveUser(email, name);
            emailSender.sendWelcomeEmail(email);
            reportService.generateReport(email);
        }
    }
}

public class EmailValidator {
    public boolean validate(String email) {
        return email.contains("@");
    }
}

public class UserRepository {
    public void saveUser(String email, String name) {
        System.out.println("Saving user...");
    }
}

public class WelcomeEmailSender {
    public void sendWelcomeEmail(String email) {
        System.out.println("Sending welcome email...");
    }
}

public class UserReportService {
    public void generateReport(String email) {
        System.out.println("Generating report...");
    }
}
```

## Your Task

Fill in the missing methods for each class.

Think carefully:

```text
What should each class own?
```

---

# Part 4 — Interfaces as Capability Contracts 🔌

Now imagine your system sends welcome messages.

At first, it only sends email.

Later, the business says:

```text
We also want SMS messages.
```

Then later:

```text
We also want WhatsApp messages.
```

If your registration system directly depends on one concrete email class, it becomes rigid.

So we introduce an interface.

## Create this interface

```java
public interface MessageSender {
    void send(String to, String message);
}
```

## Then create two implementations

```java
public class EmailSender implements MessageSender {
    public void send(String to, String message) {
        System.out.println("Email sent to " + to);
    }
}
```

```java
public class SmsSender implements MessageSender {
    public void send(String to, String message) {
        System.out.println("SMS sent to " + to);
    }
}
```

## Questions

1. What capability does `MessageSender` represent?
   1. it allows for a message to be sent via any method, it only needs the message & message service
2. Why is `MessageSender` more flexible than depending directly on `EmailSender`?
   1. MessageSender allows for many message services or even email send methods to be used based on the requirements or how many attempts have been made to send an email 
3. Which SOLID principle does this help with?
   1. Open / Closed Principle
4. How does this make the system easier to extend later?
   1. it gives developers the options without it resulting in a fragile system that breaks with any new feature

---

# Part 5 — Remove the Hardcoded Dependency ⚠️

Here is a rigid design:

```java
public class UserRegistrationService {

    private EmailSender emailSender = new EmailSender();

    public void register(String email, String name) {
        System.out.println("Registering user");
        emailSender.send(email, "Welcome, " + name);
    }
}
```

## Problem

`UserRegistrationService` is directly locked to `EmailSender`.

If we want to use SMS, WhatsApp, or a fake sender for testing, we must change this class.

## Your Task

Refactor it so that `UserRegistrationService` depends on the interface instead:

```java
public class UserRegistrationService {

    private final MessageSender messageSender;

    public UserRegistrationService(MessageSender messageSender) {
        this.messageSender = messageSender;
    }

    public void register(String email, String name) {
        System.out.println("Registering user");
        messageSender.send(email, "Welcome, " + name);
    }
}
```

## Questions

1. What changed in the design?
   1. we have made it alot more flexible 
2. What concrete class did we remove from `UserRegistrationService`?
   1. private EmailSender emailSender - new EmailSender();
3. What abstraction does it now depend on?
   1. the EmailSender interface
4. Which principle is this?
   1. DIP - Dependency Inversion Principle
5. Why is this better?
   1. because now the class doesn't care how the message is sent. it just knows it one of the services will send it, lessening the depedency

---

# Part 6 — Inheritance Truth Check 🧬

Look at this code:

```java
class Bird {
    public void fly() {
        System.out.println("Flying");
    }
}

class Penguin extends Bird {
    public void fly() {
        throw new UnsupportedOperationException("Penguins cannot fly");
    }
}
```

## Questions

1. Why does this design feel logical at first?
   1. because most birds can fly, so penguin and those alike are just edge cases
2. Why does it become a problem in code?
   1. because there is fragmentation in what the class bird can actually do
3. What promise does `Bird` appear to make?
   1. that all birds can fly
4. How does `Penguin` break that promise?
   1. it cant fly
5. Which SOLID principle is involved here?
   1. Liskov Substitution Principle

## Better Design

Complete this improved design:

```java
interface Bird {

}

interface FlyingBird {
    void fly();
}

class Eagle implements Bird, FlyingBird {

    public void fly() {
        System.out.println("Flying");
    }
}

class Penguin implements Bird {

}
```

## Final Question

Explain this sentence:

```text
Inheritance should preserve truth.
```
### answer
there should be a set of concrete capabilities that every descendant is able to do

---

# Part 7 — Composition: Building With Parts 🧩

Instead of making one class do everything, we can build a class from smaller parts.

Example:

```java
public class UserRegistrationService {

    private final EmailValidator validator;
    private final UserRepository repository;
    private final MessageSender messageSender;

    public UserRegistrationService(
        EmailValidator validator,
        UserRepository repository,
        MessageSender messageSender
    ) {
        this.validator = validator;
        this.repository = repository;
        this.messageSender = messageSender;
    }
}
```

## Questions

1. Which objects does `UserRegistrationService` use?
   1. EmailValidator, UserRepository, MessageSender
2. Is this inheritance or composition?
   1. Composition, because the class is built using other classes instead of extending one.
3. Why is this better than putting all logic inside one class?
   1. Because each class has one job to do, which makes it easier to change later
4. What does composition allow us to do?
   1. it allows us to put different classes together to create a bigger system
5. Explain this sentence:
   1. the smaller clearer pieces are the classes that do one job, and together all the smaller pieces produce a working solution.

```text
Composition lets us build bigger systems from smaller, clearer pieces.
```

---

# Part 8 — Final Reflection 🚀

Answer these in your own words.

1. What is the relationship between memory and objects?
   1. objects are stored in memory when they are created / instantiated
2. What is the relationship between objects and dependencies?
   1. objects can use other objects to help them do their jobs, so they depend on eachother
3. Why do dependencies make software harder to change?
   1. if one class depends too much on another class, changing one might mean changing lots of other code too
4. What does SOLID help us control?
   1. it helps us organise our code and be able to handle change
5. What does this sentence mean?
6. 1.

```text
Memory is where software lives.
Objects give memory shape.
SOLID keeps that shape coherent over time.
```

---

# Stretch Challenge 🌟

Design a simple checkout system using the ideas from this lesson.

Your system should include:

* `CheckoutService`
* `PaymentProcessor` interface
* `StripePaymentProcessor`
* `PaypalPaymentProcessor`
* `OrderRepository`
* `ReceiptSender`

Then answer:

1. Which classes represent responsibilities?
   1.
2. Which interface represents a capability?
   1. 
3. Where are you using composition?
   1. 
4. Where are you applying DIP?
   1. 
5. How could you add `ApplePayPaymentProcessor` without changing `CheckoutService`?
   1. 

---

# Final Compression 🧠

```text
Stack = method execution
Heap = object storage
Objects = structured memory
Classes = responsibility boundaries
Interfaces = capability contracts
Composition = controlled assembly
SOLID = survivable change
```

Use this as your map.
