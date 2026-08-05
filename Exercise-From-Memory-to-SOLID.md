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
2. Which object is created on the **heap**?
3. What does the variable `user` refer to?
4. When `main()` finishes, what can eventually happen to the `User` object? 
user is deallocated as the stack memory doesn't outlive the function (garbage collection)
5. In your own words, explain this line:

```text
An object is structured memory.
```

as an object is an instance of a class, it is an ordered creation of a blueprint with a well defined shape and behaviour, this refers to the structured element. Objects are stored on the heap, therefore outliving the function call and return

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
validating user data, saving user name and email, sending welcome email and generating use report
2. How many different reasons could this class change?
At least 4, ie one for each responsbility, however likely a lot more. For example, changes in email validation methods, changed to the welcome email format...
3. Which method relates to validation?
private void validateEmail(String email) 
4. Which method relates to persistence/database work?
private void saveUser(String email, String name)
5. Which method relates to email?
private void sendWelcomeEmail(String email)
6. Which method relates to reporting?
private void generateUserReport(String email)
7. Why could this class become difficult to maintain as the system grows?
Effectively the reason why SRP is such an important principle. This class is very sensitive to changes due to its lack of modularity 

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
public class UserRegistrationService {

} COME BACK TO

public class EmailValidator {
    public void validateEmail(String email)
}

public class UserRepository {
    public void saveUser(String email, String name)
}

public class WelcomeEmailSender {
    public void sendWelcomeEmail(String email)
}

public class UserReportService {
    public void generateUserReport(String email)
}

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
The capacity to send a message, not neccesarily refined to one particular form 

2. Why is `MessageSender` more flexible than depending directly on `EmailSender`?
With email sender, you are limited to only sending emails, whereas the message sender allows the flexibility to send SMS 

3. Which SOLID principle does this help with?
Dependency Inversion Principle, instead of depending on the concretion of 'EmailSender' we now instead have dependence on the abstraction 'MessageSender'

4. How does this make the system easier to extend later?
It facillitates the Open/ Closed principle, allowing the simplicity of adding another implementation without aving the change the system at it's core
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
        this.messageSender =  messageSender;
        
    }

    public void register(String email, String name) {
        System.out,println("Registering User");
        messageSender.send(email, 'Welcome, " + name);
}
```

## Questions

1. What changed in the design?
messaging is no longer dependent on the user registration service
2. What concrete class did we remove from `UserRegistrationService`?
The email sender component 
3. What abstraction does it now depend on?
email sender now depends on message senser
4. Which principle is this?
DIP
5. Why is this better?
If there was a change to the email sending it would be much easier to change

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
Because when we think og a type of bird, by intuitive definition, a penguin fits that category
2. Why does it become a problem in code?
creates inconsistency, reduces trust
3. What promise does `Bird` appear to make?
In the code with have the logic statement 'If a bird then can fly'
4. How does `Penguin` break that promise?
As penguin is in the class of Birds, we should expect it to satisfy the above behavioural statement, however we know penguins cannot fly which breaks the trust of the system
5. Which SOLID principle is involved here?
LSP - Abstraction must remain behaviorally truthful 


## Better Design

Complete this improved design:

```java
interface Bird {

}

interface FlyingBird {
    void fly();
}

class Eagle implements Bird, FlyingBird 
    public void fly() {
        System.out.println("Eagle is Flying");
}

class Penguin implements Bird {

}
```

## Final Question

Explain this sentence:

```text
Inheritance should preserve truth.
```
Effectively the Liskov Substituion Principle, implications inherited from a class should be correct. This may mean adding more modularity to your system in order to allow for this. 
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
EmailValidator
UserRepository 
MessageSender 
2. Is this inheritance or composition?
This is composition as inheritance would look like 'class UserRegistrationService extends EmailValidator'
3. Why is this better than putting all logic inside one class?
This means changes to one area are isolated to that area, minimising tight coupling and giving each class a focused responsibility 
4. What does composition allow us to do?
Allows us to substitute components more easily with a more modular setup
5. Explain this sentence:

```text
Composition lets us build bigger systems from smaller, clearer pieces.
```
Consider it similar to that of lego pieces, the smaller the pieces, the more creative freedom with have. We have much greater control of the system structure, and changes which impact that system when working with smaller and more clarified pieces. 
---

# Part 8 — Final Reflection 🚀

Answer these in your own words.

1. What is the relationship between memory and objects?
memory is the place where a program stores its data whilst it runs, this could be a stack or heap structure. Objects are the structures that are an instance of the class, with a defined state and behaviour. These are stored in the memory
2. What is the relationship between objects and dependencies
Dependencies act as the glue of the objects. It is important that the dependencies between classes are strategic as you don't want objects 'glued' to other objects where they shouldn't be. (To avoid creating a rigid gridlocked system)
3. Why do dependencies make software harder to change?
Dependencies manifest as certain elements of a system relying on others. With too many of these dependencies, especially where they may not be advantegous the software becomes harder to modify, and at a fragile state. Consider it like a tower of cards, smaller towers are much likely to withstand external preassure, whereas the larger the tower the more fragile the system becomes to changes.
4. What does SOLID help us control?
Solid helps us control all of the above. Essentially the dependencies,
5. What does this sentence mean?

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
