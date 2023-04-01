# Introduction to Low-Level Design:

-  _LLD stands for Low-Level Design, which is the process of designing and specifying the detailed behavior, features, and components of a software system. It is a step that comes after High-Level Design (HLD), which defines the system's overall architecture, and before actual coding takes place._


- _LLD is a critical part of the software development process because it provides a clear blueprint of how the system will be implemented. It breaks down the system's high-level design into small, manageable pieces and defines the behavior, interactions, and dependencies of each component. This detailed design serves as a reference point for developers to implement the system and for testers to validate the system's behavior._

## Here are some key reasons why we use LLD:

1. **It provides a clear blueprint:** _LLD provides a detailed specification of how the system will be implemented. This helps developers to write code that accurately reflects the design and testers to validate the system's behavior against the design._
2. **It facilitates collaboration:** _LLD helps team members to collaborate more effectively because everyone can refer to the same detailed design specification. This reduces misunderstandings and ensures that everyone is on the same page._
3. **It helps to identify potential issues early:** _LLD can help to identify potential issues early in the development process. For example, if two components have conflicting requirements or dependencies, it will become apparent during the LLD phase._
4. **It improves the quality of the final product:** _A well-designed system is one that is easy to maintain, extend, and modify. LLD helps to ensure that the system is designed in a way that is scalable, maintainable, and robust._

# Solid Principles

### _SOLID principles are a set of design principles that aim to make software systems more maintainable, flexible, and extensible. The five SOLID principles are:_
#
* [Single Responsibility Principle (SRP)](#SRP) 
* [Open-Closed Principle (OCP)](#OCP) 
* [Liskov Substitution Principle (LSP)](#LSP) 
* [Interface Segregation Principle (ISP)](#ISP) 
* [Dependency Inversion Principle (DIP)](#DIP) 
#

## Single Responsibility Principle (SRP)

`The SRP states that a class should have only one responsibility, which means it should only have one reason to change.`

`A class that handles multiple responsibilities is difficult to maintain and modify.`

### Here's an example of a class without SRP:

```java
class Order {
  public void calculateTotalPrice() {
    // ...
  }
  
  public void saveOrder() {
    // ...
  }
  
  public void sendConfirmationEmail() {
    // ...
  }
}
```
In this example, the Order class has three responsibilities: calculating the total price of an order, saving the order to a database, and sending a confirmation email.

After applying SRP, the Order class would be refactored into separate classes, each with a single responsibility. Here's an example of how the Order class might be refactored:

```java
class OrderCalculator {
  public void calculateTotalPrice() {
    // ...
  }
}

class OrderSaver {
  public void saveOrder() {
    // ...
  }
}

class EmailSender {
  public void sendConfirmationEmail() {
    // ...
  }
}
```

In this refactored code, each class has a single responsibility: OrderCalculator calculates the total price of an order, OrderSaver saves the order to a database, and EmailSender sends a confirmation email.

By applying SRP, the code is more modular and easier to maintain, test, and extend. Each class has a clear and distinct responsibility, making the code easier to reason about and change in the future.
#
## Open-Closed Principle (OCP)

`The Open-Closed Principle (OCP) states that classes should be open for extension but closed for modification.`

`This means that you should be able to add new functionality to a class without modifying its existing code.This principle promotes code reuse, maintainability, and extensibility.`

### Here is an example of a class that violates the OCP:

```java
public class Product {
    private String name;
    private double price;
    
    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }
    
    public String getName() {
        return name;
    }
    
    public double getPrice() {
        return price;
    }
    
    public double getDiscountedPrice() {
        if (price > 100) {
            return price * 0.9; // 10% discount
        } else {
            return price * 0.95; // 5% discount
        }
    }
}
```
This Product class has a method called getDiscountedPrice that calculates a discounted price based on a fixed set of rules. The problem with this implementation is that if we want to add a new discount rule, we would have to modify the getDiscountedPrice method, which violates the OCP.

To apply the OCP, we can use the strategy pattern to encapsulate the discount calculation logic in separate classes that can be added or removed without modifying the existing Product class. Here is an updated version of the Product class that uses the strategy pattern:

```java
public class Product {
    private String name;
    private double price;
    private DiscountCalculator discountCalculator;
    
    public Product(String name, double price, DiscountCalculator discountCalculator) {
        this.name = name;
        this.price = price;
        this.discountCalculator = discountCalculator;
    }
    
    public String getName() {
        return name;
    }
    
    public double getPrice() {
        return price;
    }
    
    public double getDiscountedPrice() {
        return discountCalculator.calculateDiscountedPrice(price);
    }
}

public interface DiscountCalculator {
    double calculateDiscountedPrice(double price);
}

public class TenPercentDiscount implements DiscountCalculator {
    @Override
    public double calculateDiscountedPrice(double price) {
        return price * 0.9; // 10% discount
    }
}

public class FivePercentDiscount implements DiscountCalculator {
    @Override
    public double calculateDiscountedPrice(double price) {
        return price * 0.95; // 5% discount
    }
}
```
In this updated implementation, the Product class takes a DiscountCalculator object as a parameter in its constructor. The DiscountCalculator interface defines a single method calculateDiscountedPrice that takes a product price as input and returns the discounted price. The TenPercentDiscount and FivePercentDiscount classes implement this interface and provide their own implementation of the discount calculation logic.

Now, we can create new discount calculators and pass them to the Product constructor without modifying the existing Product class. For example, we can create a new TwentyPercentDiscount class and use it like this:

```java
Product product = new Product("Some product", 200, new TwentyPercentDiscount());
double discountedPrice = product.getDiscountedPrice(); // returns 160.0
```
This updated implementation follows the OCP because it allows us to add new discount calculators without modifying the existing Product class.

#

## Liskov Substitution Principle (LSP)

`The Liskov Substitution Principle (LSP) is a principle in object-oriented programming that states that objects of a superclass should be able to be replaced with objects of its subclasses without affecting the correctness of the program.`

`In other words, a subclass should be able to substitute its parent class without changing the behavior of the program. Violating this principle can lead to unexpected and incorrect behavior in a program.`

**Let's consider an example to see how LSP can be applied in a class.**

_Suppose we have a Bird class and a Penguin class that extends the Bird class. The Bird class has a fly method that prints "The bird is flying", and the Penguin class has a swim method that prints "The penguin is swimming". However, since penguins are flightless birds, calling the fly method on a Penguin object would be incorrect. This violates the Liskov Substitution Principle._

### Here's an example implementation before applying LSP:

```java
class Bird {
    public void fly() {
        System.out.println("The bird is flying");
    }
}
 
class Penguin extends Bird {
    public void swim() {
        System.out.println("The penguin is swimming");
    }
 
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins cannot fly");
    }
}
```
In this implementation, we have overridden the fly method in the Penguin class and thrown an exception to indicate that penguins cannot fly. However, this violates LSP since it means that calling the fly method on a Penguin object would result in an exception, rather than just having no effect.

To apply LSP, we can separate the Bird class into two separate classes: FlyingBird and SwimmingBird. The FlyingBird class has a fly method, while the SwimmingBird class has a swim method. The Penguin class can then extend the SwimmingBird class without needing to override the fly method, since it does not have that behavior.

### Here's an example implementation after applying LSP:

```java
class FlyingBird {
    public void fly() {
        System.out.println("The bird is flying");
    }
}
 
class SwimmingBird {
    public void swim() {
        System.out.println("The bird is swimming");
    }
}
 
class Penguin extends SwimmingBird {
    // No need to override the fly method since penguins cannot fly
}
```

In this implementation, we have separated the Bird class into two separate classes: FlyingBird and SwimmingBird. The Penguin class then extends the SwimmingBird class without needing to override the fly method, since it does not have that behavior. This follows LSP since we can substitute a Penguin object for a SwimmingBird object without affecting the correctness of the program.

#

## Interface Segregation Principle (ISP)

`The Interface Segregation Principle (ISP) is a principle in object-oriented programming that states that clients should not be forced to depend on methods that they do not use.`

`In other words, it is better to have many smaller, specialized interfaces instead of one large interface that does everything. This allows clients to depend only on the methods they need, rather than being forced to implement or use methods they don't need.`

Let's consider an example to see how ISP can be applied in a class.

_Suppose we have a Printer class that can print documents, scan documents, and send faxes. We can define an interface for each of these behaviors: Printable, Scannable, and Faxable. However, some clients may only need to use one of these behaviors, and being forced to implement all three interfaces would violate ISP._

### Here's an example implementation before applying ISP:

```java
interface Printable {
    void print(Document document);
}
 
interface Scannable {
    void scan(Document document);
}
 
interface Faxable {
    void fax(Document document);
}
 
class Printer implements Printable, Scannable, Faxable {
    public void print(Document document) {
        // Print the document
    }
 
    public void scan(Document document) {
        // Scan the document
    }
 
    public void fax(Document document) {
        // Fax the document
    }
}
```

In this implementation, we have defined three interfaces: Printable, Scannable, and Faxable, and a Printer class that implements all three interfaces. However, this violates ISP since clients that only need to print documents would be forced to implement the Scannable and Faxable methods, even though they do not need them.

To apply ISP, we can separate the interfaces into smaller, specialized interfaces that contain only the methods that clients need. We can then create separate classes that implement each interface, and clients can choose which interfaces they need to use.

### Here's an example implementation after applying ISP:

```java
interface Printable {
    void print(Document document);
}
 
interface Scannable {
    void scan(Document document);
}
 
interface Faxable {
    void fax(Document document);
}
 
class Printer implements Printable {
    public void print(Document document) {
        // Print the document
    }
}
 
class Scanner implements Scannable {
    public void scan(Document document) {
        // Scan the document
    }
}
 
class FaxMachine implements Faxable {
    public void fax(Document document) {
        // Fax the document
    }
}
```
In this implementation, we have separated the Printer, Scanner, and FaxMachine behaviors into separate classes that each implement a single interface. Clients can then choose which interfaces they need to use, without being forced to implement methods they do not need. This follows ISP since clients are not forced to depend on methods they do not use.

#

## Dependency Inversion Principle (DIP)

`The Dependency Inversion Principle (DIP) is a principle in object-oriented programming that states that high-level modules should not depend on low-level modules. Instead, both should depend on abstractions.`

`Abstractions should not depend on details; details should depend on abstractions. This allows for flexibility and ease of maintenance, as well as promoting the use of interfaces and abstract classes.`

### Let's consider an example to see how DIP can be applied in a class.

_Suppose we have a PaymentService class that processes payments using different payment gateways. We can define a PaymentGateway interface and create different implementations for each payment gateway. However, the PaymentService class should not depend on the details of each payment gateway implementation._

### Here's an example implementation before applying DIP:

```java
interface PaymentGateway {
    void processPayment(Payment payment);
}
 
class PayPalGateway implements PaymentGateway {
    public void processPayment(Payment payment) {
        // Process payment using PayPal
    }
}
 
class StripeGateway implements PaymentGateway {
    public void processPayment(Payment payment) {
        // Process payment using Stripe
    }
}
 
class PaymentService {
    private PayPalGateway payPalGateway;
    private StripeGateway stripeGateway;
 
    public PaymentService() {
        this.payPalGateway = new PayPalGateway();
        this.stripeGateway = new StripeGateway();
    }
 
    public void processPayment(Payment payment) {
        if (payment.getGateway().equals("paypal")) {
            payPalGateway.processPayment(payment);
        } else if (payment.getGateway().equals("stripe")) {
            stripeGateway.processPayment(payment);
        }
    }
}
```
In this implementation, we have created two implementations of the PaymentGateway interface, PayPalGateway and StripeGateway, and a PaymentService class that depends on both implementations. This violates DIP since the PaymentService class depends on the details of each payment gateway implementation.

To apply DIP, we can define an abstraction that both the PaymentService class and the PaymentGateway implementations can depend on. We can then use dependency injection to provide the necessary implementation at runtime.

### Here's an example implementation after applying DIP:


```java
interface PaymentGateway {
    void processPayment(Payment payment);
}
 
class PayPalGateway implements PaymentGateway {
    public void processPayment(Payment payment) {
        // Process payment using PayPal
    }
}
 
class StripeGateway implements PaymentGateway {
    public void processPayment(Payment payment) {
        // Process payment using Stripe
    }
}
 
interface PaymentGatewayProvider {
    PaymentGateway getPaymentGateway(String gateway);
}
 
class PaymentServiceProvider implements PaymentGatewayProvider {
    private PayPalGateway payPalGateway;
    private StripeGateway stripeGateway;
 
    public PaymentServiceProvider(PayPalGateway payPalGateway, StripeGateway stripeGateway) {
        this.payPalGateway = payPalGateway;
        this.stripeGateway = stripeGateway;
    }
 
    public PaymentGateway getPaymentGateway(String gateway) {
        if (gateway.equals("paypal")) {
            return payPalGateway;
        } else if (gateway.equals("stripe")) {
            return stripeGateway;
        } else {
            throw new IllegalArgumentException("Invalid gateway: " + gateway);
        }
    }
}
 
class PaymentService {
    private PaymentGatewayProvider gatewayProvider;
 
    public PaymentService(PaymentGatewayProvider gatewayProvider) {
        this.gatewayProvider = gatewayProvider;
    }
 
    public void processPayment(Payment payment) {
        PaymentGateway gateway = gatewayProvider.getPaymentGateway(payment.getGateway());
        gateway.processPayment(payment);
    }
}

```

In this implementation, we have defined an abstraction, PaymentGatewayProvider, which provides a getPaymentGateway method that returns the

#