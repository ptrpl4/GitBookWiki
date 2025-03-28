# Make

#### Links

- [https://www.gnu.org/software/make/manual/make.html](https://www.gnu.org/software/make/manual/make.html) \
- [https://opensource.com/article/18/8/what-how-makefile](https://opensource.com/article/18/8/what-how-makefile)

## makefile

Utility Make is a build automation tool that automatically builds executable programs and libraries from source code by reading files called _makefiles_

### structure

```makefile
target … : prerequisites …
        recipe
        …
        …
```

example

`!use TAB instead spaces to avoid run errors`

```makefile
say_hello:
        @echo "Hello World"

generate:
        @echo "Creating empty text files..."
        touch file-{1..10}.txt

clean:
        @echo "Cleaning up..."
        rm *.txt
```

another

```makefile
install:
	pip3 install -r ./requirements.txt
run-locally:
	python3 ./webserver.py
```

ONE MORE

```makefile
STAGE_URL = https://url-$(TASK).k8s-stage.local\?$(SAVED_TOKEN)

MY_TOKEN = $(shell curl 'https://secure.local/MyTest/index.php?sec=Tools&ext=Token' \
	--data-raw 'json=blabla&mode=generate_token' \
	--compressed | grep -o -E 'token=([^" <]+)' | sort | uniq)
SAVE_TOKEN = $(shell curl 'https://secure.local/MyTest/index.php?sec=Tools&ext=Token' \
	--data-raw 'json=blabla&mode=generate_token' \
	--compressed | grep -o -E 'token=([^" <]+)' | sort | uniq > output.txt)
COPY_TOKEN = $(shell curl 'https://secure.local/MyTest/index.php?sec=Tools&ext=Token' \
	--data-raw 'json=blabla&mode=generate_token' \
	--compressed | grep -o -E 'token=([^" <]+)' | sort | uniq | pbcopy)

SAVED_TOKEN = $(shell grep x output.txt | tail -1)
COPYED_TOKEN = $(shell grep x output.txt | tail -1 | pbcopy)

TASK = 2020-exclusive

run:
	open -a Google\ Chrome $(STAGE_URL)
	open -a Safari $(STAGE_URL)

upd-and-save:
	$(SAVE_TOKEN)
```

## CLI

```bash
# run target
$ make say_hello
$ make install
$ make run-locally
```
