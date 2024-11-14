# Configs

CLI usage

```sh
# detailed output (-v)
$ python -m pytest --verbose
# only imortant data (-q)
$ python -m pytest --quiet
# stop after fail (--maxfail=1)
$ python -m pytest --exitfirts
# create report
$ python -m pytest --junit-xml report.xml
# filter tests by keyword
$ pipenv run python -m pytest -k keyword
```

Config files

link -  [https://docs.pytest.org/en/stable/customize.html](https://docs.pytest.org/en/stable/customize.html)

link - [https://docs.pytest.org/en/stable/reference.html#command-line-flags](https://docs.pytest.org/en/stable/reference.html#command-line-flags)

Example - pytest.ini

```
[pytest]
minversion = 6.0
addopts = -ra -q
testpaths =
    tests
    integration
```
