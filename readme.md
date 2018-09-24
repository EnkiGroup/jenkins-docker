# Jenkins 2 with Docker

Jenkins 2 inside docker containers using containers as slaves to build PHP and NodeJS

To run this project, just clone, and run the docker compose file. It will start the Jenkins Master with useful plugins and PHP 5.6, PHP 7 and NodeJS 4 slaves.

To start via docker compose:

`docker-compose.yml`

``` yaml
version: '2'
services:
    jenkins:
        image: jeffersonsouza/jenkins:alpine
        privileged: true
        restart: always
        ports:
            - "8085:8080"
            - "50000:50000"
        volumes:
            - jenkins-data:/var/jenkins_home/
            - /var/run/docker.sock:/var/run/docker.sock
    node:
        image: jeffersonsouza/jenkins:slave-node
        privileged: true
        ports:
            - 22
        volumes:
            - jenkins-data:/var/jenkins_home/
            - /var/run/docker.sock:/var/run/docker.sock
    php:
        image: jeffersonsouza/jenkins:slave-php
        privileged: true
        ports:
            - 22
        volumes:
            - jenkins-data:/var/jenkins_home/
            - /var/run/docker.sock:/var/run/docker.sock
    php56:
        image: jeffersonsouza/jenkins:slave-php5
        privileged: true
        ports:
            - 22
        volumes:
            - jenkins-data:/var/jenkins_home/
            - /var/run/docker.sock:/var/run/docker.sock
volumes:
    jenkins-data:
        driver: local
```

then run `docker-compose up -d`

## How to Config Jenkins Agent

Here are the settings to configure:

1. Number of executors: 1 or more
    - Select the number of concurrent jobs that should be allowed to run on this node
1. Remote root: Use the Jenkins workspace directory that you created
    - Example: /home/jenkins
1. labels: Add any descriptive labels you want to use
    - Examples: linux, build-x, x86
1. Select Use this node as much as possible
1. Method: Launch slave agents via SSH
1. Host: Use the static ip address of your new slave node
    - Use the docker-compose machine name. Example: dotnetcore
1. Credentials: Select the Jenkins master private ssh key
    - Examplo: user jenkins, password jenkins
1. Verification strategy: non-verifying
    - You are free to use more secure methods!
1. Select Keep this agent online as much as possible

If everything was configured correctly, the node can be brought online and Jenkins can start assigning jobs!

@todo More docs: soon. :)
