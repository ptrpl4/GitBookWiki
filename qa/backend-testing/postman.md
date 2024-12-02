# Postman

### Variables Types

```javascript
pm.globals.set("variable_key", "variable_value");
pm.collectionVariables.set("variable_key", "variable_value");
pm.environment.set("variable_key", "variable_value");
```

### Snippets

```js
// JSON value check
pm.test('Your test name', () => {
  const jsonData = pm.response.json()
  pm.expect(jsonData.value).to.eql(100)
})

// body is not null check
pm.test('Check that response body is not null', () => {
  pm.response.to.be.have.body
})

// get variable from JSON response
const jsonData = pm.response.json()
pm.environment.set('newToken', jsonData.headers['postman-token'])

// status 200 check
pm.test('Status code is 200', () => {

pm.response.to.have.status(200)
})

// Response time is less than 200ms
pm.test("Response time is less than 200ms", function () {

pm.expect(pm.response.responseTime).to.be.below(600);
});

// Items list is not empty
pm.test("Items list is not empty", function () {

var jsonData = pm.response.json();
pm.expect(jsonData.items).to.be.an('array').that.is.not.empty;
});

// Add a test to check the response schema
pm.test("Response schema is valid", function () {

var response = pm.response.json();

pm.expect(response).to.have.property('items');
pm.expect(response).to.have.property('total_items_count');
pm.expect(response.items).to.be.an('array');

response.items.forEach((item) => {
pm.expect(item).to.have.property('card_expiry_date');
pm.expect(item).to.have.property('id');
pm.expect(item).to.have.property('name');
pm.expect(item).to.have.property('pid');
pm.expect(item).to.have.property('psName');
pm.expect(item).to.have.property('switch_icon_name');
pm.expect(item).to.have.property('type');
});
});

// Content-Type header is application/json
pm.test("Content-Type header is application/json", function () {

pm.expect(pm.response.headers.get("Content-Type")).to.include("application/json");
});
```
