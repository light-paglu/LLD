# Design Principles: Quick Revision

## DRY
**Don't Repeat Yourself**

Avoid duplicating the same logic or knowledge in multiple places. Keep it in one reusable place so changes are easier and safer.

**Example:** Put a shared `calculateTotal()` method in one utility instead of rewriting the same calculation in multiple classes. If the calculation changes, you update it only once.

## KISS
**Keep It Simple, Stupid**

Prefer the simplest solution that clearly solves the problem. Avoid unnecessary complexity.

**Example:** Use a simple loop to find the largest number instead of introducing a complex framework or pattern. The code is easier to understand, test, and maintain.

## YAGNI
**You Aren't Gonna Need It**

Do not build features or abstractions until they are actually needed.

**Example:** If the application only needs email notifications, do not build support for SMS and push notifications yet. Add those options later when a real requirement appears.

## Quick Recall

- **DRY:** Remove duplication.
- **KISS:** Choose simplicity.
- **YAGNI:** Build only what is needed now.
