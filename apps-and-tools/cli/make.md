# Make

#### Links

- [https://www.gnu.org/software/make/manual/make.html](https://www.gnu.org/software/make/manual/make.html) 
- [https://opensource.com/article/18/8/what-how-makefile](https://opensource.com/article/18/8/what-how-makefile)
- [https://hexlet.io/blog/posts/what-is-a-makefile-and-how-do-i-use-it](https://hexlet.io/blog/posts/what-is-a-makefile-and-how-do-i-use-it)
- [https://makefile.site/](https://makefile.site/)

## Makefile

Utility Make is a build automation tool that automatically builds executable programs and libraries from source code by reading files called _Makefiles_

### structure

```makefile
HELLO?=World # the question mark indicates that the variable is optional. The value after assignment can be omitted

target1: # target name, you can also use kebab-case or snake_case
		command1 # it's very important to use tabs to indent 
		command2 # the commands will be executed sequentially and only if the previous one was successful
		
target2: target3 # here established the command dependency, target2 depends on target3
		command2 # target2 will be executed only if target3 command was successful
		
target3:
		command1
		
hello:
		@echo “Hi, Bob!”# command starting with @ would't be showed in output

test:
	-make test # ignoring errors
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

### Passing arguments

```shell
make staging-fetch VAR=db/dump.tgz
```

```makefile
staging-fetch:
	scp app@staging-server.dev:/path/to/app/$(VAR)/ ./
```

## CLI

```bash
# run target
$ make say_hello
$ make install
$ make run-locally
```
