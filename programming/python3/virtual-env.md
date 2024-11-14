# Virtual env

### venv

Go to project folder

```bash
# create env
$ python -m venv env
# activate env
$ source env/bin/activate
# to deactivate
$ deactivate
```

### pipenv

```sh
# on macos
brew install pipenv

# install
pip install --user pipenv
# install package
pipenv install requests
# install all packeges from Pipfile
pipenv install
# run app
pipenv run python main.py
```
