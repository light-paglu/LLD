# Access Modifiers in Java

Access modifiers control where a class, field, constructor, or method can be accessed. They are an important part of encapsulation and API design.

## Visibility Summary

| Modifier | Same class | Same package | Subclass in another package | Other packages |
| --- | --- | --- | --- | --- |
| `public` | Yes | Yes | Yes | Yes |
| `protected` | Yes | Yes | Yes, through inheritance | No |
| *(default)* | Yes | Yes | No | No |
| `private` | Yes | No | No | No |

The default level has no keyword and is also called **package-private**.

## `public`

A public member can be accessed from anywhere the class itself is accessible.

```java
public class Account {
    public void printDetails() {
        System.out.println("Account details");
    }
}
```

Use `public` for functionality that is intentionally part of a class's external API.

## `private`

A private member can be accessed only within the class that declares it. It cannot be accessed directly by subclasses or unrelated classes, even when they are in the same package.

```java
class Account {
    private double balance;

    public double getBalance() {
        return balance;
    }
}
```

Private fields are commonly used with public methods to protect and validate object state.

## `protected`

A protected member can be accessed:

- Within the declaring class.
- By other classes in the same package.
- By subclasses in other packages through inheritance.

```java
package banking;

public class Account {
    protected void calculateInterest() {
        System.out.println("Calculating interest");
    }
}
```

A subclass in another package can use the protected member as part of its inherited state or behavior:

```java
package premium;

import banking.Account;

public class PremiumAccount extends Account {
    public void updateInterest() {
        calculateInterest();
    }
}
```

A non-subclass in another package cannot access `calculateInterest()` merely because it is protected.

## Default or Package-Private Access

When no access modifier is specified, the member has package-private access. It is available only to classes in the same package.

```java
class PackageHelper {
    void help() {
        System.out.println("Available within this package");
    }
}
```

Package-private access is useful for implementation details shared by classes that belong to the same package.

## Top-Level Classes

A top-level class can be declared only as:

- `public`, making it accessible from other packages when imported.
- Package-private, making it accessible only within its package.

Top-level classes cannot be declared `private` or `protected`. Those modifiers can be used for nested classes.

## Design Guidance

Start with the most restrictive visibility that satisfies the design:

1. Use `private` for internal state and implementation details.
2. Use package-private for collaboration within one package.
3. Use `protected` when subclasses need an extension point.
4. Use `public` only for behavior that external callers should depend on.
