# UML Class Notation and Anatomy

A standard UML class is depicted as a box with **three compartments**:

```text
+----------------------------------------------------------+
|                      ClassName                           |  <- Class name
+----------------------------------------------------------+
| - privateField: String                                   |
| # protectedField: double                                 |  <- Attributes
| + publicField: int = 10                                  |
+----------------------------------------------------------+
| + publicMethod(param: String): boolean                   |
| - privateHelper(): void                                  |  <- Operations / methods
| # protectedAction(id: int): void                         |
+----------------------------------------------------------+
```

## Visibility Markers

| Symbol | Marker | Access level | Description |
|:---:|:---|:---|:---|
| `+` | **Public** | `public` | Accessible from any class in any package. |
| `-` | **Private** | `private` | Accessible only within the declaring class. |
| `#` | **Protected** | `protected` | Accessible within the class, package, and subclasses. |
| `~` | **Package** | *(default)* | Accessible only within classes in the same package. |

## Syntax for Attributes and Operations

### Attribute syntax

$$
\text{visibility } \text{name : Type } [\text{multiplicity}] = \text{DefaultValue}
$$

- **Example Java:** `public int age = 21;`
- **UML representation:** `+ age: int = 21`

### Method or operation syntax

$$
\text{visibility } \text{name(param1: Type1, param2: Type2) : ReturnType}
$$

- **Example Java:** `private boolean isAdult(int age) { return age >= 18; }`
- **UML representation:** `- isAdult(age: int): boolean`

## Special Classifier Types

### `<<interface>>`

Defines a contract with abstract method signatures and no implementation.

### `<<abstract>>`

Cannot be instantiated directly. It can contain both concrete and abstract methods. The class name is conventionally shown in *italics* in a UML diagram.

### `<<enumeration>>`

Represents a fixed set of named constants or literals.

