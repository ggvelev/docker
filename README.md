# Docker Course Labs

Labs and exercises to help you learn Docker.

Live at https://docker.courselabs.co.

# Notes

- Instructor: Elton Stoneman @ DevelopIntelligence.
- https://blog.sixeyed.com, https://eltons.show
- Docker with a read-only file system (for better security) - as far as it is applicable for concrete app.

# Links

-

# Part 1 - Containers and Images

- Provides a repeatable environment for running applications (fixes "it does not work on my machine").
- Reduces manual time-consuming routines (e.g. set up app env / server).
- Can help utilize resources way better (e.g. API server, DB server etc running on same machine, different containers,
  isolated from each other).

## /labs/containers

    1  docker version
    2  docker
    3  docker container run --help
    4  docker run apline hostname
    5  docker login
    6  docker run alpine hostname
    7  hostname
    8  docker ps
    9  docker run -it alpine hostname
    10  docker ps
    11  docker ps --help
    12  docker ps -a
    13  docker run -it alpine whoami
    14  docker run openjdk:17-alpine
    15  docker ps
    16  docker run -it openjdk:17-alpine 'java --version'
    17  docker run -it openjdk:17-alpine '/usr/bin/java --version'
    18  docker run -it openjdk:17-alpine
    19  docker ps
    20  docker ps
    21  docker run -it openjdk:17 java --version
    22  docker run -it openjdk:17-alpine java --version
    23  docker run --hostname heheyy alpine hostname
    24  docker run -it openjdk:8-jre java -version
    25  docker run -d -P --name nginx nginx:alpine
    26  docker image ls openjdk

One can also override container's IP, DNS, Hostname, env variables, compute resources (cpu/mem).

## /labs/env

    PS C:\Users\LENOVO> $htmlPath="C:\projects\docker\labs\env\html"
    PS C:\Users\LENOVO> docker run -d -p 8083:80 -v ${htmlPath}:/usr/share/nginx/html --name nginx6 nginx:alpine

    45  docker run -d -p 8031:80 --name pi kiamol/ch05-pi -m web
    46  docker inspect pi
    47  docker rm -f pi
    48  docker run -d -p 8031:80 --name pi --memory 200m --cpus 0.125 kiamol/ch05-pi -m web
    49  docker container inspect --format='Memory: {{.HostConfig.Memory}}b, CPU: {{.HostConfig.NanoCpus}}n' pi
    53  docker exec -it tls sh
    54  docker cp tls:/certs /labs/env/certs
    55  docker cp tls:/certs ${PWD}/labs/env/certs
    56  docker cp tls:/certs/server-cert.pem ${PWD}/labs/env/certs
    57  docker cp tls:/certs/server-key.pem ${PWD}/labs/env/certs
    59  docker exec tls ls
    60  docker exec tls ls -l

## /labs/images

Dockerfile - describes how to package a container application. It is the dpeloyment script for our component/app.

``` dockerfile
# we always start from existing base image, this is our starting point
FROM java

# copy some files from local machine/host to the container. Copy app binary to root dir
COPY app.jar / 

# tell the docker container what to do (e.g. execute command)
CMD ["java", "-jar", "/app.jar"]
```

``` dockerfile
FROM ubuntu

# run some commands in the container (e.g. install packages)
RUN apt-get update && apt-get install -y curl

ENV TEST_DOMAIN=docker.courselabs.co

CMD curl -sSL $TEST_DOMAIN
```

```dockerfile
# Alpine is a tiny Linux distro
FROM openjdk:17-alpine

COPY ./HelloWorld.java .

# apk is the package manager for Alpine
# this installs curl into the container image
CMD java HelloWorld.java

# tells Docker to run curl when it starts a container
#CMD curl/
```

    17  DOCKER_BUILDKIT=0  docker build -t demo/1.0-test .
    18  docker images
    19  DOCKER_BUILDKIT=0  docker build -t demo:1.0-test .
    20  docker images
    21  DOCKER_BUILDKIT=0  docker build -t demo:1.1 .
    22  docker run demo:1.1
    23  history
    24  echo $DOCKER_BUILDKIT
    25  echo $DOCKER_BUILDKIT
    26  docker build -t demo:1.1 .

## /labs/registries

TODO - complete lab because missed it for sake of meeting
play with Jfrog artifactory?

## /labs/compose

    4  docker-compose -f rng/v1.yml up
    5  docker-compose -f rng/v1.yml up
    6  docker-compose -f rng/v1.yml up -d
    7  docker-compose -f rng/v1.yml ps
    8  docker-compose -f rng/v1.yml logs
    9  docker image prune
    10  docker images
    11  docker image rm gvdockered1/demo
    12  docker image rm gvdockered1/demo*
    13  docker image rm gvdockered1/demo:*
    14  docker image rm gvdockered1/demo:1.1
    15  docker images
    16  docker image rm 44771346e252
    17  docker image rm -f 44771346e252
    18  docker ps
    19  docker-compose -f rng/v1.yml down
    20* docker-compose
    21  docker-compose -f rng/v1.yml ps
    22  docker ps
    23  docker stop
    24  docker stop rng_numbers-api_1
    25  docker stop rng_rng-web_1
    26  docker ps
    27  docker-compose -f rng/v1.yml up
    28  docker-compose -f rng/v1.yml up
    29  docker-compose -f rng/v2.yml up -d
    30  docker-compose -f rng/v2.yml logs -f
    31  docker ps
    32  docker-compose -f rng/v2.yml up -d
    33  n
    34  docker-compose -f rng/v2.yml up -d
    35  docker ps
    36  docker-compose -f rng/v2.yml up -d
    37  docker-compose -f rng/v2.yml logs -f
    38  docker-compose -f rng/v2.yml up -d
    39  docker-compose -f rng/v2.yml logs -f
    40  docker ps
    41  docker inspect rng_nginx_1
    42  docker ps
    43  docker-compose -f rng/v2.yml down
    44  docker ps
    45  docker-compose --remove-orphans
    46  docker ps
    47  docker images
    48  docker-compose -f rng/v1.yml up -d
    49  docker-compose -f nginx/docker-compose.yml up -d
    50  docker ps
    51  docker-compose -f nginx/docker-compose.yml down
    52  docker ps
    53  docker-compose -f rng/v2.yml logs
    54  docker-compose -f rng/v2.yml logs
    55  docker-compose -f rng/v2.yml logs -f
    56  docker-compose -f rng/v2.yml ps
    57  docker ps
    58  docker-compose -f rng/v2.yml up -d
    59  docker ps
    60  docker-compose -f rng/v2.yml logs -f
    61  docker-compose -f rng/v2.yml logs -f
    62  docker-compose -f rng/v2.yml up

## /labs/compose-model

    1  docker ps
    2  docker-compose -f labs/compose-model/rng/v1.yml up -d
    3  docker-compose -f labs/compose-model/rng/v1.yml logs -f
    4  docker-compose -f labs/compose-model/rng/v2.yml up -d
    5  docker container inspect --format-'{{.Config.Env}}' rng_rng-web_1
    6  docker container inspect --format='{{.Config.Env}}' rng_rng-web_1
    7  docker-compose -f labs/compose-model/rng/v1.yml logs -f
    8  docker-compose -f labs/compose-model/rng/v2.yml up -d
    9  docker container inspect --format='{{.Config.Env}}' rng_rng-web_1
    10  docker container inspect --format='{{.Config.Env}}' rng_rng-api_1
    11  docker-compose -f labs/compose-model/rng/v3.yml up -d
    12  docker container inspect --format='{{.Config.Env}}' rng_rng-api_1
    13  docker container inspect rng_rng-api_1
    14  docker-compose -f labs/compose-model/rng/v3.yml up -d
    15  docker container inspect rng_rng-api_1
    16  docker-compose -f labs/compose-model/rng/v3.yml up -d
    17  docker-compose -f labs/compose-model/rng/v3.yml up -d
    18  docker exec
    19  docker exec rng_rng-api_1 ls /app/config
    20  docker container rng_rng-api_1 exec ls /app/config
    21  docker exec
    22  docker exec -it rng_rng-api_1 ls /app/config
    23  docker exec -it rng_rng-api_1 ls
    24  docker exec -it rng_rng-api_1 ls config
    25  docker exec -it rng_rng-api_1 less config/others.json
    26  docker exec -it rng_rng-api_1 cat config/others.json
    27  cd labs/compose-model/
    28  cp ./lab/.env .
    29  cp ./lab/compose.yml ./rng/lab.yml
    30  docker-compose up -d
    31  docker compose down
    32  docker ps
    33  docker ps
    34  docker-compose -p rng-dev -f rng/v3.yml up -d
    35  docker ps
    36  docker-compose -p rng-dev -f rng/v3.yml dowdn
    37  docker-compose rng-dev down
    38  docker-compose logs
    39  docker ps
    40  docker-compose down
    41  docker-compose down --help
    42  docker-compose -f rng/v3.yml down
    43  docker ps
    44  docker ps
    45  docker-compose stop
    46  docker ps
    47  docker-compose stop rng-dev
    48  docker-compose down rng-dev
    49  docker-compose ps
    50  docker ps
    51  docker-compose -p rng-dev -f rng/core.yml -f rng/dev.yml up -d
    52  docker-compose -p rng-dev logs rng-api
    53  docker-compose -p rng-dev logs rng-web
    54  docker-compose -p rng-dev down
    55  docker ps
    56  docker-compose -p rng-dev -f rng/core.yml -f rng/dev.yml up -d
    57  docker-compose -p rng-dev -f rng/core.yml -f rng/dev.yml up -d
    58  docker-compose -p rng-dev -f labs/compose-model/rng/core.yml -f labs/compose-model/rng/dev.yml logs rng-api
    59  docker-compose -p rng-dev -f rng/core.yml -f rng/dev.yml logs rng-api
    60  docker-compose -p rng-dev -f rng/core.yml -f rng/dev.yml logs rng-api
    61  docker-compose -p rng-dev -f rng/core.yml -f rng/dev.yml logs rng-api
    62  docker-compose -p rng-dev -f rng/core.yml -f rng/dev.yml up -d
    63  docker-compose -p rng-dev -f rng/core.yml -f rng/dev.yml logs rng-api
    64  docker-compose -p rng-dev -f rng/core.yml -f rng/dev.yml up -d
    65  docker-compose -p rng-dev -f rng/core.yml -f rng/dev.yml down
    66  docker-compose -p rng-dev -f rng/core.yml -f rng/dev.yml up -d
    67  docker-compose -p rng-dev -f rng/core.yml -f rng/dev.yml logs rng-api
    68  docker-compose -p rng-dev -f rng/core.yml -f rng/dev.yml down
    69  docker-compose -p rng-dev -f rng/core.yml -f rng/test.yml up -d
    70  docker-compose -p rng-test -f rng/core.yml -f rng/test.yml up -d
    71  docker-compose -p rng-dev -f rng/core.yml -f rng/test.yml logs rng-api
    72  docker-compose version
    73  docker-compose help

## /labs/compose-build

Each docker-compose service can have a `build` clause which will specify context (folder) and dockerfile of the
service/app to build. This way we can dockerize with compose our applications. Used for building internal images.
Great for CICD projects.

You can split in separate compose files - one core.yml which uses public published service image and one build.yml which
will use local dockerfile to build the latest version of our service.

The compose build section can provide arguments as well to the dockerfile.