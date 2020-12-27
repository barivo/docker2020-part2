# 2.6 REDMINE EXAMPLE

## Key terms: 
COMPOSE_PROJECT_NAME 
Volume 
tmpfs

## and commands: 
docker-compose down 
docker exec -it db_redmine psql -U postgres 

## Compose uses the current directory as a prefix for container and volume name's so that different projects don’t clash. The prefix can be overridden withCOMPOSE_PROJECT_NAME environment variable if needed.

## Let’s put the whole “application” down now with docker-compose down ie what the "postgres" image does when it's set to let Docker manage storage is create a db_redmine container and mount it as VOLUME "24ddb..."


➜  redmine docker inspect db_redmine | grep -A 5 Mounts
        "Mounts": [
            {
                "Type": "volume",
                "Name": "24ddb3336ff715c15509a3ed5d181d0f40afb6e63e5e9fd3de95c5ffc0f84800",
                "Source": "/var/lib/docker/volumes/24ddb3336ff715c15509a3ed5d181d0f40afb6e63e5e9fd3de95c5ffc0f84800/_data",
                "Destination": "/var/lib/postgresql/data",
➜  redmine docker inspect db_redmine


# What's a Docker Volume?
## Volume's are a way to persist data when using docker and preferred to bind mounts. Volumes use linux's tmpfs (tmpfs is a temporary file storage, which appears as a mounted file-system but exists only in ram + swap)

## the redmine image's Dockerfile automatically creates an extra volume at:
VOLUME /usr/src/redmine/files

## which we add to our compose.yml as "files"so that we can manage it explicitly


## using psql instead of Adminer: We could also just use psql to interact with a postgres database with 
docker exec -it db_redmine psql -U postgres 

## (The command executes psql -U postgres inside the container) The same method can be used to create backups with pg_dump: 

docker exec db_redmine pg_dump -U postgres > redmine.dump


## the postgres login info is: postgres, postgres, example (username, database, password)

# Redmine, postgres and   Adminer 

version: '3.5'

services:
  db:
    image: postgres
    restart: unless-stopped
    environment:
      POSTGRES_PASSWORD: example
    container_name: db_redmine
    volumes:
      - database:/var/lib/postgresql/data
  redmine:
    image: redmine
    environment:
      - REDMINE_DB_POSTGRES=db
      - REDMINE_DB_PASSWORD=example
    ports:
      - 9999:3000
    volumes:
      - files:/usr/src/redmine/files
    depends_on:
      - db
  adminer:
    image: adminer
    restart: always
    environment:
      - ADMINER_DESIGN=galkaev
    ports:
      - 8083:8080

volumes:
  database:
  files:

