# 🥲 Frontend testing

Frontend testing refers to process of verifying that 
- client-side logic
- user interface
- user experience 
are functioning correctly.

## Variants of Locators

### CSS selectors

Allow you to select elements on a page based on their styles and structure.  
Can be simple (by tag, class, identifier) or complex (with a combination of conditions).

### XPath

A query language for selecting elements in XML documents such as HTML pages. It provides extensive capabilities for precise element retrieval. Among other things, it allows you to access a parent element through a child element.

**Absolute path** - starts from the root element and specifies the full path to the target element

`/html/body/div[1]/form/input`

**Relative path** - starts from the current context and is more flexible than absolute paths

Relative expressions start with a double slash (`//`)

`//div[@class='container']//button`

### Identifiers (ID)

A unique number (name) that can be used as a locator. \
Identifiers are usually the most stable locators.

### Classes and attributes

However, classes can be repetitive and changes to the page structure can affect the locator.

### Conclusion

1. Use ID if the element has a unique identifier
2. Use Name when elements have unique names, especially in forms
3. Use XPath when there are no unique IDs or names and you want to select items based on their structure, text, or other attributes

## Test Attribute Locators

Custom attributes added to HTML elements that make it easier to locate and interact with them. Help **improve stability, readability, and maintainability** of automated test scripts.

In HTML, data-* attributes allow developers to store **custom, non-visible metadata** in elements. These attributes do not affect the UI but serve as **reliable selectors** for testing.

could also be `data-test-id`, `data-testid` or `data-cy` - doesn't matter

### Examples

```html
<button id="submit-btn" class="btn primary" data-test-id="login-submit">Login</button>
```

Selenium

```python
driver.find_element(By.CSS_SELECTOR, "[data-test-id='login-submit']").click()
```

Cypress

```js
cy.get("[data-testid='login-submit']").click();
```

Playwright

```js
await page.locator("[data-test='login-submit']").click();
```
