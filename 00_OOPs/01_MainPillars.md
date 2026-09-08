# The Four Pillars of Object-Oriented Programming

The four main pillars of object-oriented programming are:

1. Polymorphism
2. Inheritance
3. Encapsulation
4. Abstraction

## 1. Polymorphism

**Polymorphism** means "many forms." The same operation can behave differently depending on how it is called or which object provides the implementation.

### Compile-Time Polymorphism: Method Overloading

Method overloading occurs when a class has multiple methods with the same name but different parameter lists. The difference can be in the number or types of parameters. The return type alone is not enough to overload a method.

```java
class Printer {
    void print(String value) {
        System.out.println(value);
    }

    void print(int value) {
        System.out.println(value);
    }
}
```

The compiler selects the overloaded method at compile time.

### Run-Time Polymorphism: Method Overriding

Method overriding occurs when a subclass provides its own implementation of an inherited method. The method signature must match the parent method, and the selected implementation depends on the actual object at runtime.

```java
class Animal {
    void sound() {
        System.out.println("Some sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Bark");
    }
}

Animal animal = new Dog();
animal.sound(); // Bark
```

## 2. Inheritance

**Inheritance** allows a child class to reuse and extend the fields and methods of a parent class.

```java
class Vehicle {
    void move() {
        System.out.println("Moving");
    }
}

class Car extends Vehicle {
    void drive() {
        System.out.println("Driving");
    }
}
```

### Types of Inheritance

#### Single Inheritance

One child class inherits from one parent class.

```text
A -> B
```

#### Multilevel Inheritance

A class inherits from a class that already inherits from another class.

```text
A -> B -> C
```

#### Hierarchical Inheritance

Multiple child classes inherit from the same parent class.

```text
    A
   / \
  B   C
```

#### Multiple Inheritance

A class inherits from more than one parent class.

```text
A     B
 \   /
   C
```

Java does not support multiple inheritance through classes because it can create ambiguity, commonly called the **diamond problem**. Java supports a form of multiple inheritance through interfaces. If two interfaces provide conflicting default methods, the implementing class must resolve the conflict by overriding the method.

#### Hybrid Inheritance

Hybrid inheritance combines multiple inheritance patterns. In Java, a class can extend only one class but can implement multiple interfaces.

```java
class SmartPhone extends Device implements Camera, MusicPlayer {
    // A single parent class and multiple interfaces
}
```

## 3. Encapsulation

**Encapsulation** bundles data and behavior inside a class and controls how that data can be accessed or modified. It supports data hiding and protects an object's state from invalid changes.

A common implementation is:

- Declare fields as `private`.
- Expose controlled access through public methods.
- Validate values before changing the object's state.

```java
class BankAccount {
    private double balance;

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Amount must be positive");
        }
        balance += amount;
    }
}
```

Getters and setters are tools for encapsulation, but they are not mandatory. A class can expose behavior such as `deposit()` without exposing a direct setter for `balance`.

## 4. Abstraction

**Abstraction** exposes only the essential details of an object while hiding its implementation details. In Java, abstraction is primarily provided through abstract classes and interfaces.

### Abstract Class

Use an abstract class when related classes should share state, constructors, or implemented behavior.

An abstract class can contain:

- Instance variables.
- Constructors.
- Abstract methods without an implementation.
- Concrete methods with an implementation.
- Static and final members.

An abstract class cannot be instantiated directly, and a Java class can extend only one class.

```java
abstract class Shape {
    abstract double area();

    void describe() {
        System.out.println("This is a shape");
    }
}

class Circle extends Shape {
    private final double radius;

    Circle(double radius) {
        this.radius = radius;
    }

    @Override
    double area() {
        return Math.PI * radius * radius;
    }
}
```

### Interface

Use an interface to define a capability or contract that unrelated classes can implement in their own ways. A class can implement multiple interfaces, which provides Java's supported form of multiple inheritance of type.

An interface can contain:

- Abstract methods.
- `default` methods with an implementation.
- `static` methods with an implementation.
- Constants, which are implicitly `public static final`.
- Private helper methods in modern Java versions.

```java
interface Flyable {
    void fly();

    default void land() {
        System.out.println("Landing");
    }
}

class Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("Flying");
    }
}
```

## Default Methods and Method Conflicts

A default method is an interface method that provides a reusable implementation. An implementing class may override it when it needs different behavior.

```java
interface Printable {
    default void show() {
        System.out.println("Printable");
    }
}

class Report implements Printable {
    @Override
    public void show() {
        System.out.println("Report");
    }
}
```

When a class inherits a method from a class and a default method with the same signature from an interface, the class implementation takes priority.

If two interfaces provide conflicting default methods, the implementing class must override the method and choose or define the desired behavior.

```java
interface A {
    default void execute() {
        System.out.println("A");
    }
}

interface B {
    default void execute() {
        System.out.println("B");
    }
}

class C implements A, B {
    @Override
    public void execute() {
        A.super.execute();
        // Or provide a completely new implementation.
    }
}
```
