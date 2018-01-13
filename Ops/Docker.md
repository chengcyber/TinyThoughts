# Download and Remove Docker Images

```
docker pull mongo
```

default search from [docker hub](https://hub.docker.com/)

```
docker pull mongo:3.0
```

```
docker pull mhart/alpine-node
```

## Manger docker images

```
docker images

docker rmi mongo
docker rmi mongo:3.0

docker rmi [IMAGE ID]
```

# Manager Container

```
docker run mongo

docker run mongo:3.0

docker run -d mongo

docker ps

docker stop [container ID]

docker ps -a

docker start [container ID]

docker stop [container ID]

docker rm [container ID]
```

# Short-lived Docker Container

```
docker run -d nginx
```

`-d` run at background

```
docker ps
```

```
docker run --rm nginx nginx -h
```

`--rm` automatically removed after container exited

## monitor short-lived container

```
docker run --rm debian hostname >> /tmp/containers
```

crontab -e

```
* * * * * /usr/local/bin/docker run --rm debian hostname >> /tmp/containers
```

```
tail -f /tmp/containers
```

# Docker Volumes

persisted data in one or more containers

## create volume

```
docker volume create --name webdata
```

## mount volume to a nginx server as web html data

```
docker run -d --name web1 -v webdata:/usr/share/nginx/html -p 8000:80 nginx
```

`-d` run at background
`-v` volume
`-p` PORT


```
curl localhost:8000
```

## change the volume data

```
docker exec web1 bash -c 'echo "foo" > /usr/share/nginx/html/index.html'
```

exec bash in web1 container: rewrite the html index file

```
curl localhost:8000
// foo
```

## volume is persisting

```
docker stop web1
docker rm web1
docker ps -a
```

container is removed

```
docker run -d --name web1 -v webdata:/usr/share/nginx/html -p 8000:80 nginx
```

```
curl localhost:8000
// foo
```

## mount same volume in multiple container

```
docker run -d --name web1 -v webdata:/usr/share/nginx/html -p 8000:80 nginx
docker run -d --name web2 -v webdata:/usr/share/nginx/html -p 8001:80 nginx
```

multiple container mount same volume

```
curl localhost:8000
// foo
curl localhost:8001
// foo
```

```
docker exec web1 bash -c 'echo "bar" > /usr/share/nginx/html/index.html'
```

```
curl localhost:8000
// bar
curl localhost:8001
// bar
```

Changes in volume, influence all container

## Manager Volumes

```
docker inspect -f '{{ .Mounts }}' web1

docker volume inspect webdata

docker volume ls

# need remove all container mount this volume
docker stop web1 web2 && docker rm web1 web2

docker volume rm webdata
```

# Prune Old Unused Docker Containers and Images

## container

```
docker ps -a

# prune all unused container
docker container prune
```

## Image

```
docker images

# prune all image without attach a container
docker image prune
```

## System

```
# prune system
# all stopped containers
# all volumes not used by at least one container
# all networks not used by at least one container
# all dangling images
docker system prune
```

```
# remove not only dangled images, but all unused images as well
docker system prune -a
```

# Docker + Node.js

## index.js

```javascript
// Load the http module to create an HTTP server
var http =  require('http');

// Configure our HTTP server to respond with Hello World to all requests
var server = http.createServer(function (request, response) {
    response.writeHead(200, { "Content-Type": "text/plain" });
    response.end("Hello, World\n");
});

// Listen on Port 8000, IP defaults to 127.0.0.1
server.listen(8000);

// Put a friendly message on the terminal
console.log('Server running at http://127.0.0.1:8000');
```

## Dockerfile

```
FROM mhart/alpine-node
COPY index.js .
EXPOSE 8000
CMD node index.js
```

## build

```
docker build -t myserver .
```

`-t` for tag


```
docker images
docker run -p 8000:8000 myserver
```

`-p` for PORT 

visit `localhost:8000`


# Docker + Nginx Proxy for Node.js App

## Node.js App

### ~/Sites/myapp

```
mkdir nodejs nginx
```

### ~/Sites/myapp/nodejs/index.js

```javascript
// Load the http module to create an HTTP server
var http =  require('http');

// Configure our HTTP server to respond with Hello World to all requests
var server = http.createServer(function (request, response) {
    response.writeHead(200, { "Content-Type": "text/plain" });
    response.end("Welcome to Node.js!\n");
});

// Listen on Port 3000, IP defaults to 127.0.0.1
server.listen(3000);

// Put a friendly message on the terminal
console.log('Server running at http://127.0.0.1:3000');
```

### ~/Sites/myapp/nodejs/Dockerfile

```
FROM mhart/alpine-node
COPY index.js .
EXPOSE 3000
CMD node index.js
```

Now, build and run

```
docker build -t foo/node .

docker run -d -p 3000:3000 --name node-app foo/node
```

`-d` run at background
`-p` PORT
`--name` name

## Nginx

### ~/Sites/myapp/nginx

```
docker run --rm -p 8000:80 nginx
```

`--rm` Automatically remove the container when it exits
`-p` PORT

### ~/Sites/myapp/nginx/default.conf

```
server {
    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote-addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $schema;
        proxy_pass http://app:3000;
    }
}
```

### ~/Sites/myapp/nginx/Dockerfile

```
FROM nginx
COPY default.conf /etc/nginx/conf.d/
```

build & run

```
docker build -t foo/nginx .

docker run -p 8000:80 --link node-app:app --name nginx-proxy foo/nginx
```

`--link` Add link to another container
`-p` PORT
`--name` name

```
curl localhost:8000
// Welcome to Node.js!
```

# *Build Your Own Custom Docker Image*

build a [Tengine Web Server](tengine.taobao.org) env in docker

```
docker run -it debian bash
```

The `-it` instructs Docker to allocate a pseudo-TTY connected to the container’s stdin; creating an interactive bash shell in the container. 

## Download install file

```
curl http://tengine.taobao.org/download/tengine-2.2.0.tar.gz > /opt/tengine-2.2.0.tar.gz
// curl: command not found
```

```
apt-get update
apt-get install -y curl
```

## Extract install file

```
cd /opt
tar xzf tengine-2.2.0.tar.gz
cd tengine-2.2.0
ls
```

## Compile and install

```
./configure
make
make install
```

### configure

error:
```
C complier cc is not found
```

solution:
```
apt-get install -y gcc
```

error:
```
the HTTP rewrite module requires the PCRE library
```

solution:
```
apt-get install -y libpcre3-dev
```

error:
```
SSL module require the OpenSSL library
```

solution:
```
apt-get install -y libssl-dev
```

SUCCESS run `./configure`

### make && make install

error:
```
make: command not found
```

solution:
```
apt-get install -y make
```

SUCCESS run `make && make install`

## Run Tengine

```
/usr/local/nginx/sbin/nginx

ps aux
```

## Build Dockerfile

### command history

```
apt-get update
apt-get install -y curl
curl htpp://tengine.taobao.org/download/tengine-2.2.0.tar.gz > /opt/tengine.2.2.0.tar.gz
cd /opt
tar xzf tengine-2.2.0.tar.gz
cd tengine-2.2.0
apt-get install -y gcc
apt-get install -y libpcre3-dev
apt-get install -y libssl-dev
apt-get install -y make
./configure
make
make install
/usr/local/nginx/sbin/nginx
```

### Dockerfile

```
FROM debian

RUN apt-get update && apt-get install -y \
    curl \
    gcc \
    libpcre3-dev \
    libssl-dev \
    make

RUN curl htpp://tengine.taobao.org/download/tengine-2.2.0.tar.gz > /opt/tengine.2.2.0.tar.gz

WORKDIR /opt

RUN tar xvf tengine-2.2.0.tar.gz

WORKDIR /opt/tengine-2.2.0

RUN ./configure

RUN make

RUN make install

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /usr/local/nginx/logs/access.log \
    && ln -sf /dev/stderr /usr/local/nginx/logs/error.log

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
```

### Build image

```
docker build -t yourname/tengine:2.2.0 .
```

```
docker images
```

```
docker run -p 8000:80 yourname/tengine:2.2.0
```

visit localhost:8000
// welcome to tengine!

# Resource

- [Docker hub](https://hub.docker.com)
- [Tutorial Video in egghead](https://egghead.io/lessons/docker-build-your-own-docker-image)

