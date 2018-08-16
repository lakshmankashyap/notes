# Docker

## Docker overview

Docker is a **containerization** platform that packages your application and all its dependencies together in the form of a docker container to ensure that your application works seamlessly in **any environment**.

For a visual explanation, think of the shipping containers used for intermodal shipping. You put your package (code and dependencies) in the container, ship it using a boat or train (laptop or cloud) and when it arrives at its destination it works (runs) just like it did when it was shipped?

## Docker container

Containers are the ready applications created from Docker Images or you can say a Docker Container is a running instance of a Docker Image and they hold the entire package needed to run the application. This happens to be the ultimate utility of Docker.

A Docker container consists of

- A Docker image
- An execution environment
- A standard set of instructions

## Docker image

Docker Image can be compared to a template which is used to create Docker Containers. They are the building blocks of a Docker Container. These Docker Images are created using the build command.

## Dockerfile

Build an image file : `Dockerfile`

```dockerfile
FROM node:8.11.1
WORKDIR /usr/src/smart-brain-api
COPY ./ ./
RUN npm install
CMD ["/bin/bash"]
```

- `WORKDIR` : What's our working directory
- `COPY ./ ./` : Copy everything in our current container
- A dockerfile can only have one `CMD` but multiple `RUN`

The container doesn't really know of the machine we have. So we have to use port forwarding to access for example `localhost:3000`

### Commands

- Build : `docker build -t awesomecontainer .`
- Run our container : `docker run -it awesomecontainer` (`-it` allows us to go "inside" an environment)
- Running in the background : `docker run -it -d awesomecontainer`
- List processes : `docker ps`
- Go back to the container : `docker exec -it [ID] bash`
- Stop the container : `docker stop [ID]`
- Exit the container : `exit`

We need to create the Dockerfile and run commands from the root directory.

**Warning :** It's important to check our containers to make sure our versions are right and all work.

## Pictures

Docker vs VM

<img src="docker_vm.png" width="500px"/>

Docker container

<img src="docker_container.png" width="500px"/>

Development

<img src="development.png" width="500px"/>

Production

<img src="production.png" width="500px"/>

## Docker compose

Docker compose is sort of a configuration file.

In `smart-brain-api` folder, create the `docker-compose.yml` :

```yml
version: '3.6'

services:
  smart-brain-api:
    container_name: backend
    # image: node:8.11.1 : not needed we build from the docker file
    build: ./
    command: npm start
    working_dir: /usr/src/smart-brain-api
    ports:
      - "3000:3000"
    volumes:
      - ./:/usr/src/smart-brain-api
```

Then we can run `docker-compose build`.

With this file, we :

- Created a service called smart-brain-api using the Dockerfile created previously
- Mapped the container to port 3000 (to have access from our laptop)
- Ran npm start in the container to start our service
- Add a volumes section that allows us to persist data and bind the structure in the docker image to the host's directory

**Next command :** `docker-compose run smart-brain-api`

`docker-compose run` isn't really useful since we have generally multiple services. We use instead `docker-compose up`. It allows us to bring up all the services defined in the `docker-compose` file.

We can run `docker-compose down` to remove our containers.

We can also use `docker-compos exec`. For example : `docker-compose exec smart-brain-api bash` (We need to "up" the containers in the background for that : `docker-compose up -d`)

**Note :** everytime the `.yml` file changes, we need to run `docker-compose build` again.

### Adding postgres to smart-brain-api

```yml
version: '3.6'

services:
  # Backend API
  smart-brain-api:
    container_name: backend
    # image: node:8.11.1 : not needed we build from the docker file
    build: ./
    command: npm start
    working_dir: /usr/src/smart-brain-api
    environment: # environmental variables
      POSTGRES_URI: postgres://sally:secret@postgres:5432/smart-brain-docker
    links:
      - postgres
    ports:
      - "3000:3000"
    volumes:
      - ./:/usr/src/smart-brain-api

  # Postgres
  postgres:
    environment:
      POSTGRES_USER: sally
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: smart-brain-docker
      POSTGRES_HOST: postgres # not localhost!
    image: postgres
    ports:
      - "5432:5432" # default port for postgres
```

This still won't work because we need to create the tables, so we need to do the following :

1. Create a folder called `postgres` in `smart-brain-api` and add a `Dockerfile`
2. Insert these lines :

The syntax may look weird but it's taken from the postgres image documentation.

3. Create a folder in `postgres` called `tables` and add two files : `login.sql` and `users.sql`

`login.sql`

```sql
BEGIN TRANSACTION;

CREATE TABLE login (
  id serial PRIMARY KEY,
  hash varchar(100) NOT NULL,
  email text UNIQUE NOT NULL
);

COMMIT;
```

`users.sql`

```sql
BEGIN TRANSACTION;

CREATE TABLE users (
  id serial PRIMARY KEY,
  name VARCHAR(100),
  email text UNIQUE NOT NULL,
  entries BIGINT DEFAULT 0,
  joined TIMESTAMP NOT NULL
);

COMMIT;
```

4. Create a folder called `seed` and insert `seed.sql` 

```sql
BEGIN TRANSACTION;

INSERT INTO users (name, email, entries, joined) values('Jessie', 'jessie@gmail.com', 5, '2018-01-01');
INSERT INTO login (hash, email) values('HASH_TO_ADD_HERE', 'jessie@gmail.com');
```

5. Add a `deploy_schemas` sql file :

```sql
-- Deploy fresh database tables
\i 'docker-entrypoint-initdb.d/tables/users.sql' -- Execute scripts
\i 'docker-entrypoint-initdb.d/tables/login.sql'
\i 'docker-entrypoint-initdb.d/seed/seed.sql'
```

6. Add the Dockerfile

```Dockerfile
FROM postgres:10.3

ADD /tables/ /docker-entrypoint-initdb.d/tables/ 
ADD /seed/ /docker-entrypoint-initdb.d/seed/ 
ADD deploy_schemas.sql /docker-entrypoint-initdb.d/
```

## Resources

- [Docker Hub](https://hub.docker.com/)
- [A Beginner-Friendly Introduction to Containers, VMs and Docker](https://medium.freecodecamp.org/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b)
- [docker-compose build](https://docs.docker.com/compose/reference/build/)
- [docker-compose run](https://docs.docker.com/compose/reference/run/)
- [docker-compose up](https://docs.docker.com/compose/reference/up/)
- [Creating tables in SQL](http://joshualande.com/create-tables-sql)
- [Postgres image documentation](https://hub.docker.com/_/postgres/)
- [In case nothing is working...](https://github.com/aneagoie/smart-brain-boost-api-dockerized/commit/a7efbdd56c9fbfb29700c3398286dedd38e56166)