# QA techniques

## Test approaches
### Shift-left strategy

QA is involved in the early stages of development (when its reasonable).

Classical way - to be involved only when developing is done and feature is ready for testing.

### Test components

- test data
- test logic
- application driver (transport, framework)

Example for api test
- test_data/test_dataGen.py
- tests/user_registration.py
- requests lib in python (for https requests)

Example for UI test
- test-data/test-dataGen.js
- tests/user-registration.spec.js
- playwright browser calls (for requests to browser)

## Design (Structural) Patterns

#### links

- [Николай Алименков — Паттерны проектирования в автоматизации тестирования (ru)](https://youtu.be/EnooA2kEhY0)

### POM (Page object model)

Page data and components described in separate files/classes and folders.

```
../page-components/home-page
../page-components/top-menu-page
../shared-components/login-widget

../tests/test.spec.ts
```

### Factory pattern

Define test data creation logic in separate files/classes that you can create it calling it with necessary data and parameters.

Typical use case - Generating dynamic test data or test objects for multiple configurations.

### Strategy pattern

Idea - use different "strategy" based on test needs.
Your tests will call() some strategy based on test needs.

- In case if test needs to test some user features in UI it doesn't need to go trough UI registration every time. It can use API-strategy, or DB-strategy of user creation logic if main point is not to check registration for this case.

But this strategy should only be used in cases where the logic we want to skip has already been covered in other tests.

### Singleton pattern

Ensures only one instance of a WebDriver, API client, or database connection exists throughout test execution.
