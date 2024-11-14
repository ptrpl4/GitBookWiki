# Testing

## Api Tests based on pytest and requests

### Structure

project\_folder

* **tests** - folder for tests
  * **test.py** - test files
  * def test\_name
* **config.py** - config data
* readme.md - helper

### common modules

requests 

pprint 

```python
from pprint import pprint


def pretty_print(msg, indent=2):
    print()
    pprint(msg, indent=indent)
```

json

pytest 