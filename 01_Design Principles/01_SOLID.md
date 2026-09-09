# SOLID Principles

SOLID is a set of five object-oriented design principles that help make software easier to understand, extend, test, and maintain.

## Quick Revision

| Letter | Principle | Main idea |
| --- | --- | --- |
| **S** | Single Responsibility Principle | A class should have one reason to change. |
| **O** | Open/Closed Principle | Software should be open for extension and closed for modification. |
| **L** | Liskov Substitution Principle | A subtype should be usable wherever its base type is expected. |
| **I** | Interface Segregation Principle | Clients should not depend on methods they do not use. |
| **D** | Dependency Inversion Principle | High-level policy should depend on abstractions, not low-level details. |

---

## S - Single Responsibility Principle (SRP)

> A class should have one, and only one, reason to change.

A class should focus on one responsibility. This does not necessarily mean that it can have only one method; it means that its methods should belong to the same area of responsibility.

### How to apply it

- Separate unrelated responsibilities into different classes.
- Give each class a clear purpose.
- Keep business logic, input/output, persistence, and presentation concerns separate.

### Example

A restaurant may have separate responsibilities for:

- Chef: preparing food
- Waiter: serving customers
- Cleaner: maintaining cleanliness
- Receptionist: managing reservations

Similarly, an online compiler can separate these responsibilities:

- Driver: accepting the code and selecting the language
- Test runner: running all test cases
- Repository: storing results
- Response handler: returning the result to the user

```java
class Order {
	private final List<String> items;

	Order(List<String> items) {
		this.items = items;
	}

	List<String> getItems() {
		return items;
	}
}

class OrderCalculator {
	double calculateTotal(Order order) {
		// Calculate the order total.
		return 0.0;
	}
}

class OrderRepository {
	void save(Order order) {
		// Persist the order.
	}
}
```

`OrderCalculator` calculates totals, while `OrderRepository` persists orders. A database change should not require changing the calculation logic.

### Revision question

> If this class changes, are there multiple unrelated reasons that could cause the change?

---

## O - Open/Closed Principle (OCP)

> Software entities should be open for extension but closed for modification.

Existing, tested code should not need frequent changes whenever a new variation is introduced. New behavior should usually be added through new implementations or extensions.

### How to apply it

- Define an interface or abstraction for behavior that may vary.
- Add new implementations instead of adding more conditions to existing code.
- Use polymorphism or the Strategy pattern where appropriate.

### Example: Tax calculation

Define a tax-calculation interface and create separate implementations for each country:

- `IndiaTaxCalculator`
- `UsTaxCalculator`
- `UkTaxCalculator`

Adding support for another country should involve adding a new implementation rather than repeatedly modifying a large `TaxCalculator` class.

### Example: Charging adapters

A charging service can depend on a `ChargingAdapter` interface. New adapter types can be added without changing the service that uses the abstraction.

```java
interface TaxCalculator {
	double calculate(double amount);
}

class IndiaTaxCalculator implements TaxCalculator {
	public double calculate(double amount) {
		return amount * 0.18;
	}
}

class UsTaxCalculator implements TaxCalculator {
	public double calculate(double amount) {
		return amount * 0.10;
	}
}

class Checkout {
	double totalWithTax(double amount, TaxCalculator taxCalculator) {
		return amount + taxCalculator.calculate(amount);
	}
}
```

Adding `UkTaxCalculator` extends the behavior without modifying `Checkout`.

### Revision question

> Can I add a new variation by adding a class or implementation instead of modifying stable code?

---

## L - Liskov Substitution Principle (LSP)

> Objects of a subtype should be replaceable with objects of its base type without breaking the correctness of the program.

A child class must honor the behavior promised by its parent class or interface. It may implement the logic differently, but it should not violate the expected contract.

### How to apply it

- Use inheritance only when the child truly represents a valid subtype.
- Prefer small, behavior-focused interfaces.
- Ensure overriding methods preserve the expected inputs, outputs, and guarantees.
- Avoid forcing a subclass to reject operations supported by its parent.

### Example: Notifications

Suppose an application depends on a `Notification` abstraction. Email, SMS, and WhatsApp notifications should all support the same expected operation, such as `send(message)`.

If the WhatsApp implementation uses a different method name or behaves in a way that breaks the caller's assumptions, it cannot safely substitute for `Notification`.

```java
interface Notification {
	void send(String message);
}

class EmailNotification implements Notification {
	public void send(String message) {
		System.out.println("Email: " + message);
	}
}

class SmsNotification implements Notification {
	public void send(String message) {
		System.out.println("SMS: " + message);
	}
}

class AlertService {
	void alert(Notification notification, String message) {
		notification.send(message);
	}
}
```

`AlertService` can use either implementation through the same `Notification` contract.

### Common warning sign

If a subclass throws an exception for a normal operation that the parent type promises to support, the design may violate LSP.

### Revision question

> Could every implementation be passed to existing client code without surprising failures or extra type checks?

---

## I - Interface Segregation Principle (ISP)

> No client should be forced to depend on methods it does not use.

Large interfaces often force implementing classes to provide irrelevant methods. Smaller, focused interfaces make implementations simpler and reduce coupling.

### How to apply it

- Split large interfaces into smaller, role-specific interfaces.
- Let clients depend only on the operations they need.
- Model different user roles separately when their capabilities differ.

### Example: Ride-sharing application

Instead of one large `User` interface containing every operation, define focused interfaces such as:

- `RiderActions`: request a ride, cancel a ride, view trip history
- `DriverActions`: accept a ride, start a trip, end a trip

A rider should not be forced to implement driver operations, and a driver should not be forced to implement rider-specific operations.

```java
interface RiderActions {
	void requestRide();
	void cancelRide();
}

interface DriverActions {
	void acceptRide();
	void endRide();
}

class Rider implements RiderActions {
	public void requestRide() { }
	public void cancelRide() { }
}

class Driver implements DriverActions {
	public void acceptRide() { }
	public void endRide() { }
}
```

`Rider` does not need to implement driver operations, and `Driver` does not need to implement rider operations.

### Revision question

> Does this implementation contain empty, unsupported, or meaningless methods because the interface is too broad?

---

## D - Dependency Inversion Principle (DIP)

> High-level modules should not depend on low-level modules. Both should depend on abstractions.
>
> Abstractions should not depend on details. Details should depend on abstractions.

High-level code contains business rules. Low-level code contains implementation details such as databases, APIs, file systems, or third-party services. The business rules should depend on interfaces rather than directly creating or calling those details.

### How to apply it

- Define an abstraction at the boundary between policy and implementation.
- Inject dependencies instead of constructing them inside high-level classes.
- Use Dependency Injection and the Strategy pattern.

### Example: Recommendation engine

A Netflix-like recommendation service can depend on a `RecommendationStrategy` interface. Different strategies can be supplied without changing the recommendation service:

- Recent content strategy
- Trending content strategy
- Genre-based strategy

The recommendation service contains the high-level workflow, while each strategy contains the algorithmic detail.

```java
interface RecommendationStrategy {
	List<String> recommend(String userId);
}

class TrendingStrategy implements RecommendationStrategy {
	public List<String> recommend(String userId) {
		return List.of("Trending title");
	}
}

class RecommendationService {
	private final RecommendationStrategy strategy;

	RecommendationService(RecommendationStrategy strategy) {
		this.strategy = strategy;
	}

	List<String> recommend(String userId) {
		return strategy.recommend(userId);
	}
}

RecommendationService service =
		new RecommendationService(new TrendingStrategy());
```

The service depends on `RecommendationStrategy`, not on a specific recommendation algorithm. The strategy can be replaced with a recent-content or genre-based implementation.

### Revision question

> Can I replace the database, API client, or algorithm without changing the high-level business logic?

---

## How the Principles Work Together

A maintainable design often uses several SOLID principles at once:

1. **SRP** separates responsibilities.
2. **OCP** makes variable behavior extensible.
3. **LSP** keeps implementations interchangeable.
4. **ISP** keeps abstractions focused.
5. **DIP** connects high-level logic to low-level implementations through abstractions.

SOLID is a set of guidelines, not a requirement to add an interface or class for every small piece of code. Apply the principles when they reduce coupling, clarify responsibilities, or make change safer.

## One-Line Memory Aid

**S**eparate responsibilities, **O**pen for extension, **L**et subtypes substitute, **I**solate interfaces, and **D**epend on abstractions.
