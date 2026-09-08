# Generics and Wildcards in Java

## Generics

**Generics** allow classes, interfaces, and methods to work with different types while preserving compile-time type safety. They improve reusability and reduce the need for explicit type casting.

Generic type arguments must be reference types. Primitive types such as `int`, `boolean`, and `char` cannot be used directly, but their wrapper classes can:

| Primitive | Wrapper type |
| --- | --- |
| `int` | `Integer` |
| `boolean` | `Boolean` |
| `char` | `Character` |
|

Java's autoboxing and unboxing make wrapper types convenient to use with primitive values.

```java
List<Integer> numbers = new ArrayList<>();
numbers.add(10); // int is autoboxed to Integer
int value = numbers.get(0); // Integer is unboxed to int
```

## Generic Methods

A generic method declares its type parameter before the return type. The compiler can infer the type from the argument passed to the method.

```java
class Display {
    public <T> void show(T element) {
        System.out.println(element);
    }
}

Display display = new Display();
display.show("Hello World"); // T is inferred as String
display.show(500);           // T is inferred as Integer
```

A generic method can also be static because its type parameter belongs to the method itself.

```java
class Utility {
    public static <T> T first(T left, T right) {
        return left;
    }
}

String text = Utility.first("A", "B");
Integer number = Utility.first(10, 20);
```

## Generic Classes

A generic class declares one or more type parameters that can be used for its fields, constructors, and methods.

```java
class Box<T> {
    private final T object;

    Box(T object) {
        this.object = object;
    }

    public T getObject() {
        return object;
    }
}

Box<Integer> integerBox = new Box<>(15);
Box<String> stringBox = new Box<>("Hello");
```

The compiler knows the type returned by `getObject()`, so callers do not need to cast it manually.

## Multiple Type Parameters

A generic class or method can declare multiple type parameters. By convention, common names include `T` for type, `K` for key, and `V` for value.

```java
class Pair<K, V> {
    private final K key;
    private final V value;

    Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() {
        return key;
    }

    public V getValue() {
        return value;
    }
}

Pair<String, Integer> pair = new Pair<>("Age", 15);
```

## Wildcards

A **wildcard** represents an unknown type. It is written with `?` and is useful when a method should work with a generic type without requiring the caller to specify that exact type.

### Unbounded Wildcard: `<?>`

Use `<?>` when the method only needs to inspect values or perform operations that are valid for every possible type.

```java
static void printItems(List<?> items) {
    for (Object item : items) {
        System.out.println(item);
    }
}
```

A `List<?>` can refer to a `List<String>`, `List<Integer>`, or any other `List<T>`. Values cannot generally be added to it because the actual element type is unknown; only `null` is safe to add.

```java
List<String> names = List.of("Asha", "Ravi");
List<Integer> scores = List.of(90, 85);

printItems(names);
printItems(scores);
```

An unbounded wildcard is not something to avoid. It is appropriate when the method does not need to add values or depend on a specific element type.

### Upper-Bounded Wildcard: `<? extends T>`

Use `<? extends T>` when a collection produces values of type `T` or a subtype of `T`. This is useful for reading values.

```java
static double total(List<? extends Number> numbers) {
    double result = 0;
    for (Number number : numbers) {
        result += number.doubleValue();
    }
    return result;
}

List<Integer> integers = List.of(1, 2, 3);
List<Double> decimals = List.of(1.5, 2.5);

total(integers);
total(decimals);
```

Values cannot be safely added to a collection declared with `? extends Number`, because the exact subtype is unknown.

### Lower-Bounded Wildcard: `<? super T>`

Use `<? super T>` when a collection consumes values of type `T` or a supertype of `T`. This is useful for adding values.

```java
static void addScores(List<? super Integer> scores) {
    scores.add(90);
    scores.add(85);
}

List<Number> numbers = new ArrayList<>();
addScores(numbers);
```

Values can be read from a `List<? super Integer>` only as `Object`, because the exact supertype is unknown.

## PECS Guideline

A useful rule for choosing between bounded wildcards is **PECS**:

- **Producer Extends**: use `? extends T` when a structure produces values for your code to read.
- **Consumer Super**: use `? super T` when a structure consumes values that your code wants to add.

Use a type parameter such as `<T>` instead of a wildcard when the method needs to relate multiple parameters or preserve the exact type in its return value.
