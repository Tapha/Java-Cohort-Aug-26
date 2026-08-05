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
The main() method and the local variable user
2. Which object is created on the **heap**?
The new user object
3. What does the variable `user` refer to?
user refers to the user object on the heap
4. When `main()` finishes, what can eventually happen to the `User` object?
it can become eligible for garbage collection
5. In your own words, explain this line:

```text
An object is structured memory.
```
an object is memory organised into named information such as email and name 

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
it validates,saves,emails and generates reports
2. How many different reasons could this class change?
it has at least 4 different reasons to change
3. Which method relates to validation?
Validation:validateEmail()
4. Which method relates to persistence/database work?
Database:saveUser()
5. Which method relates to email?
sendWelcomeEmail()
6. Which method relates to reporting?
generateUserReport()
7. Why could this class become difficult to maintain as the system grows?
It could become difficult to maintain because it has too many different jobs.

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

}

public class EmailValidator {

}

public class UserRepository {

}

public class WelcomeEmailSender {

}

public class UserReportService {

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

}
```

```java
public class SmsSender implements MessageSender {

}
```

## Questions

1. What capability does `MessageSender` represent?
The ability to send a message
2. Why is `MessageSender` more flexible than depending directly on `EmailSender`?
Because email, SMS, WhatsApp and other senders can all implement it.
3. Which SOLID principle does this help with?
Mainly the Dependency Inversion Principle and the Open/Closed Principle.
4. How does this make the system easier to extend later?
We can add a new sender class without changing the registration service.

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
        // your code here
    }

    public void register(String email, String name) {
         System.out.println("Registering user");
        messageSender.send(email, "Welcome, " + name);
    }
}
```

## Questions

1. What changed in the design?
UserRegistrationService no longer creates its own sender. The sender is passed into the constructor.
2. What concrete class did we remove from `UserRegistrationService`?
EmailSender
3. What abstraction does it now depend on?
The MessageSender interface
4. Which principle is this?
DependancyInversionPrinciple
5. Why is this better?
Because the service can now use email, SMS, WhatsApp or a fake sender for testing without changing the UserRegistrationService code.

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
Because a penguin is a type of bird.
2. Why does it become a problem in code?
The Bird class says all birds can fly, but penguins cannot.
3. What promise does `Bird` appear to make?
That every bird has working fly() behaviour.
4. How does `Penguin` break that promise?
Its fly() method throws an error instead of flying.
5. Which SOLID principle is involved here?
The Liskov Substitution Principle.

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
        System.out.println("Eagle flying");
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
When a child class inherits from a parent, everything the parent promises should still be true for the child.
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
emailvalidator , userrepository , messagesender
2. Is this inheritance or composition?
composition
3. Why is this better than putting all logic inside one class?
each class has one clear job, so the code is easier to understand and change.
4. What does composition allow us to do?
it allows us to build a larger service using smaller objects and replace those objects when needed
5. Explain this sentence:

```text
Composition lets us build bigger systems from smaller, clearer pieces.
```
It means a large program can be made by connecting several small classes, where each class performs one simple job.
---

# Part 8 — Final Reflection 🚀

Answer these in your own words.

1. What is the relationship between memory and objects?
Objects are stored in memory while the program runs.
2. What is the relationship between objects and dependencies?
Objects depend on other objects to help them do their jobs.
3. Why do dependencies make software harder to change?
If classes are tightly connected, changing one can affect others.
4. What does SOLID help us control?
If classes are tightly connected, changing one can affect others.
5. What does this sentence mean?
Memory holds the program, objects organise it, and SOLID keeps it organised as it grows.

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
2. Which interface represents a capability?
3. Where are you using composition?
4. Where are you applying DIP?
5. How could you add `ApplePayPaymentProcessor` without changing `CheckoutService`?

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
