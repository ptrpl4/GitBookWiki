# PyTest

#### example

```python
def test_code_function():
    a = 3
    b = 4
    c = 0
    assert a + b == c
```

with requests

```python
import pytest
import requests

@pytest.mark.api
@pytest.mark.duckduckgo
def test_duckduckgo_instant_answer_api():

  # Arrange
  url = 'https://api.duckduckgo.com/?q=python+programming&format=json'

  # Act
  response = requests.get(url)
  body = response.json()

  # Assert
  assert response.status_code == 200
  assert 'Python' in body['AbstractText']
```

#### links

- doc - [https://docs.pytest.org/en/stable/contents.html](https://docs.pytest.org/en/stable/contents.html)
- tutor - [https://testautomationu.applitools.com/pytest-tutorial/](https://testautomationu.applitools.com/pytest-tutorial/)
