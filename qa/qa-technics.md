## QA

## Test approaches
### Shift Left

QA is involved in the early stages of development (when its reasonable).

Classical way - to be involved only when developing is done and feature is ready for testing.

## Test components

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

## Structural Patterns

### POM (Page object model)

Page data and components described in separate files and folders.

```
../page-components/home-page
../page-components/top-menu-page
../shared-components/login-widget

../tests/test.spec.ts
```
