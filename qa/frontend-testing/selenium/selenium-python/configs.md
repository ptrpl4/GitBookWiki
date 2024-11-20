# Configs

## config.json

Example:

```python
config.json

{
    "browser": "Headless Chrome",
    "implicit_wait": 10
}
```

## conftest.py (pytest)

Example:

```python
tests/conftest.py

"""
This module contains shared fixtures.
"""

import json
import pytest
import selenium.webdriver
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver import ChromeOptions

@pytest.fixture
def config(scope='session'):
    # ^means that function runs one time per 'session' and add value to each case
    
    
    # Read File
    with open('config.json') as config_file:
        config = json.load(config_file)
    
    # Assert values are acceptable
    assert config['browser'] in ['Firefox', 'Chrome', 'Headless Chrome']
    assert isinstance(config['implicit_wait'], int)
    assert config['implicit_wait'] > 0

    # Returnn config
    return config

@pytest.fixture
def browser(config):

    # Initialize WebDriver instance
    if config['browser'] == 'Firefox':
        s = Service('./geckodriver')
        browser = webdriver.Chrome(service = s)
    elif config['browser'] == 'Chrome':
        s = Service('./chromedriver')
        browser = webdriver.Chrome(service = s)
    elif config['browser'] == 'Headless Chrome':
        opts = selenium.webdriver.ChromeOptions()
        opts.add_argument('headless')
        s = Service('./chromedriver')
        browser = webdriver.Chrome(service=s, options=opts)

    # Add 10 sec wait
    browser.implicitly_wait(10)

    # Return the test driver instance
    yield browser

    # Quit the test driver instance
    browser.quit()
```
