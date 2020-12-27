# Frontend
FROM ubuntu:16.04

WORKDIR /mydir
RUN apt-get update && apt-get install -y curl git 

RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt-get install -y nodejs

RUN git clone https://github.com/docker-hy/frontend-example-docker.git

RUN cd  frontend-example-docker && npm install

RUN npm install -g serve

WORKDIR frontend-example-docker 

RUN node_modules/.bin/webpack --mode production

# ENV API_URL=http://localhost:8000
# note the LACK of PORT number below! which maps the "/api/ping" request to 
# "/api/ping:8000"  ie the backend
ENV API_URL=http://localhost/api 

RUN npm run build

ENTRYPOINT ["/usr/bin/serve", "-s", "-l","5000","dist"]

# CMD ["npm", "start"]
