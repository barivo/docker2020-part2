# 2.5

#### see https://medium.com/@caysever/docker-compose-network-b86e424fad82
#### to ispect the local networks use: 
docker networks ls

#### the default network is named after the directory which contains the docker-compose.yml file
docker network inspect exercise-23_default


#### change the backend/Dockerfile to include the network name/ address of the redis container:

ENV FRONT_URL=http://localhost:3000 \
  REDIS=redis-network

####  REDIS=exercise-23_redis_1  ## this is the default CONTAINER name set up by docker (from docker network ls :   "Name": "exercise-23_redis_1") which docker uses to find the redis network


#### add a redis service to the docker-compose.yml
  redis:
    container_name: redis-network  # docker CONTAINER name
    build:
      context: ./redis
      dockerfile: Dockerfile
    ports:
      - "6379:6379"



