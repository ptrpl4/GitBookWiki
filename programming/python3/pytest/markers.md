# Markers

## Custom Markers

Step 1 add marker to tests

```python
@pytest.mark.webtest
def test_send_http():
    pass  # perform some webtest test for your app


def test_something_quick():
    pass
```

Step 2 add markers to pytest.ini file

```
[pytest]
markers =
    webtest: mark a test as a webtest.
    slow: mark test as slow.
```

Step 3 run marked tests

```sh
# run marked tests
$ pytest -v -m webtest
# run unmarked tests
$ pytest -v -m "not webtest"
```

## Default Markers

### parametrize

link - [https://docs.pytest.org/en/stable/parametrize.html#parametrize-basics](https://docs.pytest.org/en/stable/parametrize.html#parametrize-basics)

Example

```python
import pytest

# A parametrized test function


@pytest.mark.parametrize("test_input,expected", [("3+5", 8), ("2+4", 6), ("6*9", 42)])
def test_eval(test_input, expected):
    assert eval(test_input) == expected
    
# Or

products = [
  (2, 3, 6),            # postive integers
  (1, 99, 99),          # identity
  (0, 99, 0),           # zero
  (3, -4, -12),         # positive by negative
  (-5, -5, 25),         # negative by negative
  (2.5, 6.7, 16.75)     # floats
]

@pytest.mark.parametrize('a, b, product', products)
def test_multiplication(a, b, product):
  assert a * b == product
```
