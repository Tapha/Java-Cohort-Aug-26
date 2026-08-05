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
   a. the Class User
2. Which object is created on the **heap**?
   a. User user
3. What does the variable `user` refer to?
   a. an instatioated object of the class User, named 'user'
4. When `main()` finishes, what can eventually happen to the `User` object?
   a. garbage collection should delete it
5. In your own words, explain this line:
   

```text
An object is structured memory.
```

    a. when you create an object from a class, since that is now a real thing, (not a blueprint as opposed to the class), it is something is stored in memory, and therefore now an integral part of running the program

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
   a. for creating a new user, and all the introductory processess associated with that
2. How many different reasons could this class change?
   a. a few depending on there are other things that need to be done in the intro processes (e.g. generate unique id based on name)
3. Which method relates to validation?
   a. validateEmail
4. Which method relates to persistence/database work?
   a. SaveUser
5. Which method relates to email?
   a. sendWelcomeEmail
6. Which method relates to reporting?
   a. generateUserReport
7. Why could this class become difficult to maintain as the system grows?
   a. validating the email might take a while if the database is massive, or email server may be very busy, each method will have different time requirements when instantiating registerUser

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
    EmailValidator.run
    UserRepository.run
    WelcomeEmailSender.run
    UserReportService.run

}

public class EmailValidator {
    input(string) - email
    checks if it inclides 1 x @ and ends with (com, co.uk, me etc)
    output(Boolean) - True(a real email), False()
}

public class UserRepository {
    input(string) - email, name
    output - checks and saves User into database
    (handles duplicate user? unique id (email+name hash))
    output (True if successful?)
}

public class WelcomeEmailSender {
    input(string) - email
    assuming EmailValidator returns True
    sends email via email server
    output(True if successfully send)
}

public class UserReportService {
    input(string) - email
    creates an empty report using another class?
    output(True if successful)
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
   a. it allows for a message to be sent via any method, it only needs the message & message service
2. Why is `MessageSender` more flexible than depending directly on `EmailSender`?
   a. MessageSender allows for many message services or even email send methods to be used based on the requirements or how many attempts have been made to send an email 
3. Which SOLID principle does this help with?
   a. Open / Closed Principle
4. How does this make the system easier to extend later?
   a. it gives developers the options without it resulting in a fragile system that breaks with any new feature

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
        MessageSender.EmailSender
    }

    public void register(String email, String name) {
        sout("Registering user");
        MessageSender.EmailSender(email, "Welcome, " + name);
    }
}
```

## Questions

1. What changed in the design?
   a. we have made it alot more flexible 
2. What concrete class did we remove from `UserRegistrationService`?
   a. private EmailSender emailSender - new EmailSender();
3. What abstraction does it now depend on?
   a. the EmailSender interface
4. Which principle is this?
   a. DIP - Dependency Inversion Principle
5. Why is this better?
   a. it breaks down the problem into abstract (sending a message) and which message service to actually use, and therefore less dependant on one implementation

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
   a. because most birds can fly, so penguin and those alike are just edge cases
2. Why does it become a problem in code?
   a. because there is fragmentation in what the class bird can actually do
3. What promise does `Bird` appear to make?
   a. that all birds can fly
4. How does `Penguin` break that promise?
   a. it cant fly
5. Which SOLID principle is involved here?
   a. Liskov Substitution Principle

## Better Design

Complete this improved design:

```java
interface Bird {

}

interface FlyingBird {
    void fly();
}

class Eagle implements Bird, FlyingBird {

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
   a. 
2. Is this inheritance or composition?
   a. 
3. Why is this better than putting all logic inside one class?
   a.
4. What does composition allow us to do?
   a.
5. Explain this sentence:

```text
Composition lets us build bigger systems from smaller, clearer pieces.
```
    a.

---

# Part 8 — Final Reflection 🚀

Answer these in your own words.

1. What is the relationship between memory and objects?
   a. 
2. What is the relationship between objects and dependencies?
   a. 
3. Why do dependencies make software harder to change?
   a. 
4. What does SOLID help us control?
   a. 
5. What does this sentence mean?

```text
Memory is where software lives.
Objects give memory shape.
SOLID keeps that shape coherent over time.
```
    a. 

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
   a.
2. Which interface represents a capability?
   a. 
3. Where are you using composition?
   a. 
4. Where are you applying DIP?
   a. 
5. How could you add `ApplePayPaymentProcessor` without changing `CheckoutService`?
   a. 

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
