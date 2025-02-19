# k6 scripts

#### links

official examples - [https://k6.io/docs/examples](https://k6.io/docs/examples)

```javascript
// Single request
import http from "k6/http";

export const options = {
  iterations: 1,
};

export default function () {
  const response = http.get("https://test-api.k6.io/public/crocodiles/");
}
```

additional example

```js
import { check } from 'k6';
import http from 'k6/http';

// Define the options for the load test
export let options = {
  stages: [
    { duration: '1m', target: 10 }, // up to 10 VUs over 1 m
    { duration: '2m', target: 10 }, // maintain 10 VUs for 2 m
    { duration: '1m', target: 0 },  // ramp-down to 0 VUs over 1 m
  ],
};

export default function () {
  // Send an HTTP GET request
  let res = http.get('https://example.com');

  // Check that the response status is 200
  check(res, {
    'status is 200': (r) => r.status === 200,
    'response time is under 200ms': (r) => r.timings.duration < 200,
  });
}
```