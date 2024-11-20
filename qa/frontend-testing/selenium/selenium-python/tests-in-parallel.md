# Tests in Parallel

## Pytest-xdist

```bash
# install pytest-xdist
$ pipenv install pytest-xdist
# run
# -m module-name
# -n (or --numprocesses) option
$ pipenv run python -m pytest -n 3
#pytest will spawn a number of workers processes equal to the number of available CPUs
$ pytest -n auto
```
