# Waiting

doc - [https://selenium-python.readthedocs.io/waits.html](https://selenium-python.readthedocs.io/waits.html)

doc - [https://www.selenium.dev/documentation/webdriver/waits/](https://www.selenium.dev/documentation/webdriver/waits/)

### FluentWait

```python
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions 


driver = Firefox()
driver.get("http://somedomain/url_that_delays_loading")
wait = WebDriverWait(driver, 10, poll_frequency=1, ignored_exceptions=[ElementNotVisibleException, ElementNotSelectableException])
element = wait.until(expected_conditions.element_to_be_clickable((By.XPATH, "//div")))
```
