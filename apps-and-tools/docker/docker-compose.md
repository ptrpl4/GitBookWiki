# Docker Compose

An Orchestration Tool for defining and running multi-container Docker applications on a single host machine.

#### links

- [compose](https://docs.docker.com/compose/)
- [reference](https://docs.docker.com/reference/compose-file/)

## Structure

Docker Compose has several top-level elements that provide a structured approach to defining and managing multi-container applications.

### Elements

#### services

Docker defines each container as a separate service with their own configuration options. The image the container should use, any environment variables that need to be established, and any ports that need to be opened can all be specified in services section.

```yaml
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"

  db:
    image: postgres:13
    environment:
      POSTGRES_USER: example
      POSTGRES_DB: exampledb
```

##### image

image attribute specifies the Docker image that serves as the foundation for your services, including the operating system, libraries, and dependencies necessary for its functionality

##### build

build attribute enables customization of your services with a build context and an optional Dockerfile. The end result is a tailored container image made for the application's specific requirements.

##### environment

environment attribute sets container environment variables that you can use for configuring application settings or providing dynamic information essential for the service during runtime.

##### ports

enables external access to specific endpoints for communication and interaction with external systems.

syntax - `<host>:<container>`

```yaml
ports:
      - 80:80
```

##### depends_on

depends_on attribute defines service dependencies in Docker Compose to make sure services start in the correct order. This is necessary to ensure availability for interaction with other services.

```yaml
version: '3'
services:
  webapp:
    image: 'nginx:latest'
    ports:
      - '80:80'
    depends_on:
      - database
  database:
    image: 'mysql:latest'
    environment:
      MYSQL_ROOT_PASSWORD: my-secret-password
    ports:
      - '3306:3306'
```

##### networks

facilitate isolated communication between services.

```yaml
version: '3'
services:
  webapp:
    image: nginx:latest
    ports:
      - "80:80"
    networks:
      - mynetwork
  database:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: my-secret-pw
    networks:
      - mynetwork

networks:
  mynetwork:
    driver: bridge
```

##### command

command attribute overrides the default container image command and allows you to specify custom commands for the service startup.

#### volumes

volumes mounts directories or named volumes from the host machine into service containers to provide persistent data sharing and storage across services.

syntax
`<host dir>:<container dir>`

```yaml
version: "3"
services:
  app:
    image: myapp
    volumes:
      - ./data:/app/data
```

Shared volume

```yaml
version: "3"
services:
  app1:
    image: nginx
    volumes:
      - shared_volume:/app/data
  app2:
    image: busybox
    command: ["tail", "-f", "/dev/null"]
    volumes:
      - shared_volume:/app/data

volumes:
  shared_volume:
    driver: local
    driver_opts:
      type: nfs
      o: addr=192.168.1.10,nolock,soft,rw
      device: ":/path/to/shared_folder"
```

##### driver_opts

allows customization to optimize volume management for app's needs and storage setup. If you don't provide `driver_opts` for a volume, docker implements the default options which depend on the chosen driver

#### configs

useful when you need to parameterize your services or share common configurations across multiple instances

```yaml
version: "3"
services:
  app:
    image: myapp
    configs:
      - app_config

configs:
  app_config:
    file: ./config.ini # stores actual config data and located in the same directory as the Docker Compose file
```

#### secrets

secrets offer a hidden vault for containers to access when required, do not show up in logs. 

```yaml
version: "3.1"
services:
  db:
    image: mysql
    environment:
      - MYSQL_ROOT_PASSWORD_FILE=/run/secrets/db_root_password
      - MYSQL_USER=myuser
      - DB_USER_PASSWORD_FILE=/run/secrets/db_user_password
    secrets:
      - db_root_password
      - db_user_password

secrets:
  db_root_password:
    file: ./db_root_password.txt
  db_user_password:
    environment: "DB_USER_PASSWORD"
```

#### extensions

### Examples

`docker-compose.yml`

example:

```yaml
version: '3' # optional since v1.27.0
services:
  my-app:
    image: ci-cd-app:1.0
    ports:
      - 3000:3000
  mongodb:
    image: mongo
    ports:
      - 27017:27017
    environment:
      - MONGO_INITDB_ROOT_USERNAME=admin
      - MONGO_INITDB_ROOT_PASSWORD=password
    volumes:
      - mongo-data:/data/db
  mongo-express:
    image: mongo-express
    restart: always # fixes MongoNetworkError when mongodb is not ready when mongo-express starts
    ports:
      - 8080:8081
    environment:
      - ME_CONFIG_MONGODB_ADMINUSERNAME=admin
      - ME_CONFIG_MONGODB_ADMINPASSWORD=password
      - ME_CONFIG_MONGODB_SERVER=mongodb
volumes:
  mongo-data:
    driver: local
```

web app + redis

```yaml
version: "3.9"  # optional since v1.27.0
services:
  web:
    build: .
    ports:
      - "8000:5000"
    volumes:
      - .:/code
      - logvolume01:/var/log
    depends_on:
      - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
```
server, db, app

```yaml
version: '3' # optional since v1.27.0
services:
  web:
    image: nginx
    ports:
      - "8020:80"
    volumes:
      - ./web:/usr/share/nginx/html
    db:
      image: postgres
      environment:
        POSTGRES_USER: admin
        POSTGRES_PASSWORD: admin
        POSTGRES_DB: test_db
    app:
      image: my-custom-website
      environment:
        DATABASE_URL: postgres://admin:admin@db:4321/test_db
        depends_on:
          - db
```

## Best practice

- you can link secrets with environment variables to add another layer of security to your application's configuration
## Commands

notes:

**network** - you don't need to specify docker-network using compose. Compose will run containers in new created docker-network

```sh
# up
docker compose up -d
# stop
docker compose stop
# restart
docker compose start
# remove
docker compose down
```

## Dockge

- https://github.com/louislam/dockge

```bash
# Create directories that store your stacks and stores Dockge's stack
mkdir -p /opt/stacks /opt/dockge
cd /opt/dockge

# Download the compose.yaml
curl https://raw.githubusercontent.com/louislam/dockge/master/compose.yaml --output compose.yaml

# Start the server
docker compose up -d

# If you are using docker-compose V1 or Podman
# docker-compose up -d
```

### Homeassistant

```
version: "3"
services:
  app:
    container_name: homeassistant
    image: ghcr.io/home-assistant/home-assistant:stable
    restart: unless-stopped
    volumes:
      - /opt/stacks/hass/hass-config:/config
      - /etc/localtime:/etc/localtime:ro
      - /run/dbus:/run/dbus:ro
    privileged: true
    network_mode: host
```
