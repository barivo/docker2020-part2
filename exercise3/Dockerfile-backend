# Backend
FROM ubuntu:16.04

WORKDIR /mydir

RUN apt-get update && apt-get install -y curl git 

RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt-get install -y nodejs

RUN git clone https://github.com/docker-hy/backend-example-docker.git

RUN cd  backend-example-docker && npm install

# WORKDIR /mydir/backend-example-docker or
WORKDIR backend-example-docker 

ENV FRONT_URL=http://localhost:3000 \
    REDIS=redis-26 \
    DB_USERNAME=postgres \
    DB_NAME=postgres \
    DB_PASSWORD=example \
    DB_HOST=postgres-26
    #  REDIS=exercise-23_redis_1

ENTRYPOINT ["/usr/bin/npm", "start"]

