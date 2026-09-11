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

1. Which parts of this code are likely stored on the **stack**? Memory address locations for args, user, email and name. As well as main().
2. Which object is created on the **heap**? The object User, created by 'new'.
3. What does the variable `user` refer to? Points to the actual address of the memory location for 'User'.
4. When `main()` finishes, what can eventually happen to the `User` object? Can become eventually eligible for garbage collection if no longer in use.
5. In your own words, explain this line: Objects define the shape within the memory allocation, so when declaring an object we declare its shape and store a reference pointer to the actual location within the memory of this object.

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

1. What is this class responsible for? User registration for an email sub of some sort.
2. How many different reasons could this class change? 4 different ways via each method.
3. Which method relates to validation? ValidateEmail()
4. Which method relates to persistence/database work? saveUser()
5. Which method relates to email? sendWelcomeEmail()
6. Which method relates to reporting? generateUserReport()
7. Why could this class become difficult to maintain as the system grows? Has 4 dependencies which breaks the single responsibility principle in SOLID, this damages maintainability as you gain traits such as coupling.

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
    public void RegisterUser(String email, String name)
}

public class EmailValidator {
    public void ValidateEmail(String email)
}

public class UserRepository {
    public void saveUser(String email, String name)
}

public class WelcomeEmailSender {
    public void SendWelcome(String email)
}

public class UserReportService {
    public void GenerateReport(String email)
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

1. What capability does `MessageSender` represent? Stores the contact details.
2. Why is `MessageSender` more flexible than depending directly on `EmailSender`? 'to' can be altered to be say a phone number OR email, its a more abstract method.
3. Which SOLID principle does this help with? Dependency inversion principle as modules now depend on abstractions.
4. How does this make the system easier to extend later? Because it's abstract you could add further features to this interface without having to remove or alter any of the proceeding features as they are already very abstract.

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
        System.out.println("User registering");
        messageSender.send(email, "Welcome, " + name);
    }
}
```

## Questions

1. What changed in the design? Using a more abstracted variable messageSender as opposed to a more specific dependent such as emailSender.
2. What concrete class did we remove from `UserRegistrationService`? EmailSender
3. What abstraction does it now depend on? messageSender
4. Which principle is this? Dependency inversion principle
5. Why is this better? Means features can be added without having to edit and change older lower level features and risk causing errors down the line.

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

1. Why does this design feel logical at first? It removes the issue specific to penguins but the parent class would otherwise still work for other birds.
2. Why does it become a problem in code? Violates the Liskov substitution principle, so the penguin subclass should be replaceable with the bird parent class without breaking anything but of course it causes an error as penguins violate the method within the parent class.
3. What promise does `Bird` appear to make? Anything that is a bird will inherit the ability to fly.
4. How does `Penguin` break that promise? penguins are famously flightless.
5. Which SOLID principle is involved here? Liskov Substitution principle

## Better Design

Complete this improved design:

```java
interface Bird {

}

interface FlyingBird {
    void fly();
}

class Eagle implements Bird, FlyingBird {
    @Override
    public void fly() {
        System.out.println("Eagle flies");
    }
}

class Penguin implements Bird {

}
```

## Final Question

Explain this sentence: When a subclass inherits a method or "trait" from it's parent class, this shouldn't in any way violate any property inherent to the subclass, i.e. a dog is an animal, it shouldn't therefore inherit a method such as makes sound "moo" for example.

```text
Inheritance should preserve truth.
```

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

1. Which objects does `UserRegistrationService` use? EmailValidator, UserRepository, MessageSender
2. Is this inheritance or composition? Composition
3. Why is this better than putting all logic inside one class? Less coupling, easier to test etc. Related to the SRP and DIP. 
4. What does composition allow us to do? Independent testing and easier implementation of changes to code.
5. Explain this sentence: We can decompose larger classes into smaller classes responsible for single methods that makes their individual functions easier to understand and test as it becomes easier to see where our program breaks down.

```text
Composition lets us build bigger systems from smaller, clearer pieces.
```

---

# Part 8 — Final Reflection 🚀

Answer these in your own words.

1. What is the relationship between memory and objects? Objects are the structure of data in the memory in which we manipulate and define when writing code. Objects are an instance of a class where a class is what defines this shape.
2. What is the relationship between objects and dependencies? Dependencies are simply the relationship between objects where one object may require another to complete its purpose.
3. Why do dependencies make software harder to change? If you change object A and object B depends on A then you end up effecting both objects rather than just the one.
4. What does SOLID help us control? Scalability, maintainability and how our code will perform and change with time. We use SOLID to prevent software entropy.
5. What does this sentence mean? When writing code to produce software we are simply just manipulating memory to store that data we write into it. Objects are instances of class' in which define the shape of the data in memory, the objects correspond to the actual structure of this data. SOLID prevents the unwanted change and degrading of this shape over time as things naturally change. 

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

```java

public interface PaymentProcessor{
    void processPayment(double amount);
}

public class StripePaymentProcessor implements PaymentProcessor{
    @Override           //used as a safety net to ensure syntax is correct fyi
    public void processPayment(double amount) {
        System.out.println("Payment processing:" + amount);
    }
}

public class PaypalPaymentProcessor implements PaymentProcessor{
    @Override
    public void processPayment(double amount){
        System.out.println("Payment processing:" + amount);
    }
}

public class OrderRepository {

    public void saveOrder(String orderId) {
        System.out.println("Saving order: " + orderId);
    }
}

public class ReceiptSender {

    public void sendReceipt(String email) {
        System.out.println("Receipt sent to: " + email);
    }
}

public class CheckoutService {

    private final PaymentProcessor paymentProcessor; //prevents other classes from accessing this variable and final means that once the variable is assigned, it can no longer be reassigned. i.e. when we take a payment processor, this can't be changed.  
    private final OrderRepository orderRepository;
    private final ReceiptSender receiptSender;

    public CheckoutService(
            PaymentProcessor paymentProcessor,
            OrderRepository orderRepository,
            ReceiptSender receiptSender
    ) {
        this.paymentProcessor = paymentProcessor; //take the input object and assign it to this object.
        this.orderRepository = orderRepository;
        this.receiptSender = receiptSender;
    }

    public void checkout(String orderId, double amount, String email) {

        paymentProcessor.processPayment(amount);

        orderRepository.saveOrder(orderId);

        receiptSender.sendReceipt(email);

        System.out.println("Checkout complete");
    }
}



```

Then answer:

1. Which classes represent responsibilities? PaymentProcessor, OrderRepository and ReceiptSender
2. Which interface represents a capability? PaymentProcessor as it means that checkoutService doesn't have to care about the specific paymentProcessor to work.
3. Where are you using composition? CheckoutService as it calls other objects to complete its function.
4. Where are you applying DIP? PaymentProcessor parent class implementation for the payment method subclasses.
5. How could you add `ApplePayPaymentProcessor` without changing `CheckoutService`? Simply implement another class of the name ApplePaymentProcessor in the same way we did for stripe and paypal.

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
