# Object-Oriented Programming Basics

Object-oriented programming (OOP) organizes software around **objects** that combine state and behavior. Java implements OOP through classes, objects, constructors, and access/modifier keywords.

## Class

A **class** is a blueprint or structure that defines the data and behavior an object can have.

```java
class Student {
    String name;

    void study() {
        System.out.println(name + " is studying");
    }
}
```

## Object

An **object** is an instance of a class. Creating an object brings the class structure to life with actual data.

```java
Student student = new Student();
student.name = "Asha";
student.study();
```

## Constructors

A **constructor** initializes an object when it is created. A constructor:

- Has the same name as its class.
- Has no return type, including `void`.
- Can be overloaded.
- Can call another constructor in the same class with `this()`.
- Can call a parent-class constructor with `super()`.
- Cannot be `static`, `final`, or `abstract`.
- May use `return` only to exit early; it cannot return a value.

When a subclass object is created, the parent constructor executes before the subclass constructor. If no explicit parent constructor call is present, Java inserts `super()` when possible.

### Types of Constructors

#### 1. Default Constructor

If no constructor is declared, the Java compiler provides a **no-argument default constructor**. It initializes fields with their default Java values, such as `0`, `false`, or `null`.

```java
class Account {
    // Compiler provides: Account() {}
}
```

> A no-argument constructor written by the developer is more precisely called a no-argument or custom constructor.

#### 2. Custom No-Argument Constructor

A developer-defined no-argument constructor can assign custom initial values.

```java
class Account {
    String type;

    Account() {
        type = "Savings";
    }
}
```

#### 3. Parameterized Constructor

A parameterized constructor accepts values while the object is created.

```java
class Account {
    String type;

    Account(String type) {
        this.type = type;
    }
}

Account account = new Account("Current");
```

#### 4. Copy Constructor

Java does not provide copy constructors automatically, but a class can define one that accepts another object of the same class.

```java
class Account {
    String type;

    Account(Account original) {
        this.type = original.type;
    }
}
```

#### 5. Private Constructor

A private constructor prevents code outside the class from directly creating objects. It is commonly used in utility classes or as part of a singleton design, although a private constructor alone does not make a class a singleton.

```java
class Utility {
    private Utility() {
        // Prevent instantiation
    }
}
```

## `this` Keyword

`this` refers to the current object instance. It can be used to:

- Distinguish instance fields from parameters with the same name.
- Chain constructors with `this()`.
- Return the current object for method chaining.
- Pass the current object as an argument.

```java
class User {
    private String name;

    User(String name) {
        this.name = name;
    }

    User rename(String name) {
        this.name = name;
        return this;
    }
}
```

`this` cannot be used in a static context because static members belong to the class rather than to a particular object.

## `static` Keyword

`static` makes a member belong to the class itself rather than to individual instances. Static fields are shared by all objects of that class, and static methods can be called without creating an object.

```java
class Counter {
    static int count = 0;

    Counter() {
        count++;
    }

    static int getCount() {
        return count;
    }
}
```

In Java, `static` can be used with:

- Variables (class fields)
- Methods
- Initialization blocks
- Nested classes

A top-level class cannot be `static`; only a nested class can be declared static.

## `final` Keyword

`final` prevents further modification or extension, depending on where it is used:

| Applied to | Effect |
| --- | --- |
| Variable | Can be assigned only once. |
| Method | Cannot be overridden by a subclass. |
| Class | Cannot be extended. |

```java
final class Constants {
    static final double PI = 3.14159;
}
```

A `final` reference cannot point to a different object, but the object it references may still be mutable.
