# Dockerfile

- [full reference](https://docs.docker.com/reference/dockerfile/)
- [linter](https://github.com/michaellzc/vscode-hadolint)
- [Auditing](https://docs.docker.com/scout/)

Text file that contains a set of instructions and commands used to build a Docker image. It is the blueprint for creating a Docker container

Key components:

1. **Base Image**: The starting point for the Docker image.
2. **Instructions**: The set of commands to be executed during the image build process, such as `RUN`, `COPY`, `ENV`, `CMD`, etc.
3. **Metadata**: Additional information about the image, such as the maintainer, exposed ports, and volume mounts.

## Syntax

```dockerfile
# Comment
INSTRUCTION arguments
```

## .dockerignore

exclude files or directories from the build context. This helps avoid sending unwanted files and directories to the builder, improving build speed, especially when using a remote builder.

```dockerignore
# Section name
node_modules
bar
foo

# Another section
.foo
.bar
```

## Commands

```shell
# find dockerfile in current dir and tag build as my-app:1.0
docker build -t my-app:1.0 .

# run created build
docker run my-app:1.0
```

## Examples

### Node app + mongo

```dockerfile
FROM node

ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=password

RUN mkdir -p /home/app

# copy from host to conainer
COPY ./app /home/app

# set default dir so that next commands executes in /home/app dir
WORKDIR /home/app

# will execute npm install in /home/app because of WORKDIR
RUN npm install

# no need for /home/app/server.js because of WORKDIR
CMD ["node", "server.js"]
```

### Python + Pipenv example <a href="#remove-a-container-using-the-cli" id="remove-a-container-using-the-cli"></a>

```dockerfile
FROM python:3.9-slim as base

# Setup env
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONFAULTHANDLER 1

FROM base AS python-deps

# Install pipenv and compilation dependencies
RUN pip install pipenv
RUN apt-get update && apt-get install -y --no-install-recommends gcc

# Install python dependencies in /.venv
COPY Pipfile .
COPY Pipfile.lock .
RUN PIPENV_VENV_IN_PROJECT=1 pipenv install --deploy

FROM base AS runtime

# Copy virtual env from python-deps stage
COPY --from=python-deps /.venv /.venv
ENV PATH="/.venv/bin:$PATH"

# Create and switch to a new user
RUN useradd --create-home appuser
WORKDIR /home/appuser
USER appuser

# Install application into container
COPY . .

# Run the application
ENTRYPOINT ["python", "-m", "http.server"]
CMD ["--directory", "directory", "8000"]
```

## Instructions

### FROM

FROM: - Specifies the base image
	  `FROM <image>[:<tag>]`
	  `FROM ubuntu:20.04 as base`

### RUN

Executes commands or a set of commands in a new layer on top of the current image during an image build time

#### options

`--mount` option [to create mounts](https://docs.docker.com/engine/reference/builder/#run---mount) that you can access at the build time to bind files, store cache, etc.

types:
- **bind**: binds files or directories to the build container
- **cache**: caches directories for compilers and package managers
- **tmpfs**: for mounting tmpfs in the build container
- **secret**: gives access to secure files
- **ssh**: gives access to SSH keys via SSH agents

```dockerfile
FROM ubuntu:22.04

LABEL author=HyperUser

RUN --mount=type=cache,target=/var/cache/apt/archives \
  apt-get update -y \
  && apt-get upgrade -y \
  && apt-get install iputils-ping -y \
  && apt-get install net-tools -y

ENTRYPOINT ["/bin/bash"]
```

This configuration can speed up the build process because Docker can use cache from the target directory in case it needs to rebuild the `RUN` layer in later builds.

### COPY

COPY: - Copies files or directories from the host filesystem to the image.

Syntax
```dockerfile
COPY [OPTIONS] <src> ... <dest>
COPY [OPTIONS] ["<src>", ... "<dest>"]
```

link - https://docs.docker.com/reference/dockerfile/#copy

Example
```dockerfile
FROM ubuntu:22.04
 
LABEL author=HyperUser
  
COPY demo.txt /demo/ # copy demo.txt to demo folder in root directory

# COPY . /app/ # copy everything from working dir to app folder

# COPY /settings/config / # copy config file to root folder
 
ENTRYPOINT ["ls", "-l", "demo/demo.txt"]
```

### ADD

ADD: - Similar to `COPY`, but also supports remote URLs and unpacking compressed files.

- The source can be a **remote URL** including git repositories. 
- The source can also be a `tar` archive of `identity`, `gzip`, `bzip2`, or `xz`compression formats.

Syntax
```dockerfile
ADD <src> <dest>
ADD myapp.tar.gz /app
```

### CMD

CMD: - Specifies the default command to run when a container is started.
	  [cmd vs entrypoint](https://docs.docker.com/reference/dockerfile/#understand-how-cmd-and-entrypoint-interact)
	  `CMD ["executable","param1","param2"...]`
	  `CMD ["python3", "app.py"]`
	  or
	  `CMD echo Hello Students. # Run command By default /bin/sh -c`

### ENTRYPOINT

ENTRYPOINT: - Configures a container to run as an executable.
	  [cmd vs entrypoint](https://docs.docker.com/reference/dockerfile/#understand-how-cmd-and-entrypoint-interact)
	  `ENTRYPOINT ["executable","param1","param2"]`
	  `ENTRYPOINT ["python3", "app.py"]`

examples
```dockerfile
ENTRYPOINT ["ls","bin", "-l"]
```

### ENV

lets you declare environment variables to use inside images

ENV: - Sets environment variables.
	  `ENV <key>=<value>`
	  `ENV APP_ENV=production`
both _key_ and _value_ can be set with or without double quotes.

example
```dockerfile
FROM ubuntu:22.04
 
LABEL author=HyperUser 

ENV HOST_FILE=demo.txt
ENV IMAGE_DESTINATION="/tmp"
COPY $HOST_FILE $IMAGE_DESTINATION

ENTRYPOINT ["ls", "/tmp"]
```

#### commands

```shell
docker run -e ENV=my_env -it --name hs-ubuntu-v1 ubuntu:v1 
```

### EXPOSE

tcp - default protocol.\
to publish port use flag `-p`

EXPOSE: - Informs Docker that the container listens on the specified network ports at runtime.
	  `EXPOSE <port> [<port>/<protocol>]`
	  `EXPOSE 80`

example
```dockerfile
FROM ubuntu:22.04

LABEL author=HyperUser

EXPOSE 80 # not specified 80/tcp
EXPOSE 80/udp # 80/udp

ENTRYPOINT ["/bin/bash"]
```

#### commands

```bash
# check exposed ports on running containers
docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Names}}\t{{.Ports}}"
```

### VOLUME

VOLUME: - Creates a mount point with the specified path and marks it as holding externally mounted volumes.
	  `VOLUME ["/path/to/dir"]`
	  `VOLUME ["/data"]`

#### commands

```shell
docker volume create sharedvol

# Start a MySQL and mount the shared volume for data persistence
docker run -d --name mysql-container -v sharedvol:/var/lib/mysql mysql

# Start WordPress and mount the same volume to access the MySQL data
docker run -d --name wordpress-container -v sharedvol:/var/www/html wordpress
```

### USER

default user - root

Sets the user name (or UID) and optionally the user group (or GID) to use as the default user and group for the remainder of the current stage. The specified user is used for RUN instructions and at runtime, runs the relevant ENTRYPOINT and CMD commands.

example
```dockerfile
FROM ubuntu:22.04

LABEL author=HyperUser

USER bin

ENTRYPOINT ["whoami"]
```

### WORKDIR

default working directory is the `/`. can be set more than once.

Sets the working directory for any subsequent `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, and `ADD` instructions.
	  `WORKDIR /path/to/workdir`
	  `WORKDIR /app`

```dockerfile
FROM ubuntu:22.04

LABEL author=HyperUser

WORKDIR /etc # creates /etc folder
WORKDIR /etc/pupupu/lol # creates /etc/pupupu/lol

ENTRYPOINT ["pwd"]
```

### LABEL

define [labels to specify metadata](https://docs.docker.com/engine/reference/builder/#label)
      `LABEL key=value`
      `LABEL "application_environment"="development"`

### ARG

The `ARG` instruction defines a variable that users can pass at build-time to the builder with the `docker build` command using the `--build-arg <varname>=<value>` flag.

syntax
`ARG <name>[=<default value>]`

example
```dockerfile
ARG VERSION
FROM ubuntu:$VERSION
```

#### commands

```bash
$ docker build --build-arg VERSION=22.04 -t ubuntu:v1 .
```

## Best practices

### Instructions

- avoid the `apt-get upgrade` command. It can bring upgrades that can affect the environment you use.
- Minimise RUN instruction layers
- use alpine images if applicable
- Remove unnecessary packages after installation
- use `--no-install-recommends` for `apt-get install`
- Use the exec form

## Single-stage build

Uses one FROM statement, final image could be big in case of many layers or unnecessary data.

## Multi-stage build

Uses multiple FROM statements. Final build will be based on image from last FROM statement.

```dockerfile
# Stage 1: Building the application
FROM node:14 as builder
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
RUN npm run build

# Stage 2: Creating the final image
FROM nginx:1.21
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
