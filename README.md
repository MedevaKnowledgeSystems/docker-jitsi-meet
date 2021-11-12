# Jitsi Meet on Docker

![](resources/jitsi-docker.png)

[Jitsi](https://jitsi.org/) is a set of Open Source projects that allows you to easily build and deploy secure videoconferencing solutions.

[Jitsi Meet](https://jitsi.org/jitsi-meet/) is a fully encrypted, 100% Open Source video conferencing solution that you can use all day, every day, for free — with no account needed.

This repository contains the necessary tools to run a Jitsi Meet stack on [Docker](https://www.docker.com) using [Docker Compose](https://docs.docker.com/compose/).

## Installation

The installation manual is available [here](https://jitsi.github.io/handbook/docs/devops-guide/devops-guide-docker).

### Kubernetes

If you plan to install the jitsi-meet stack on a Kubernetes cluster you can find tools and tutorials in the project [Jitsi on Kubernetes](https://github.com/jitsi-contrib/jitsi-kubernetes).

## TODO

* Support container replicas (where applicable).
* TURN server.

Jist Setup
+++++++++++

Server Setup:-

> Install Docker and docker-compose.

```
> Install Docker
Ref: https://docs.docker.com/engine/install/ubuntu/
> Install  docker-compose
Ref: https://docs.docker.com/compose/install/
```

Jitsi Setup:-

```
Ref: https://jitsi.github.io/handbook/docs/devops-guide/devops-guide-docker
```
1, Clone Latest stable Jitsi from official Git repo

```
Ref: https://github.com/jitsi/docker-jitsi-meet/releases/tag/stable-6433
```
2, Once done create a .env file by copying and adjusting env.example.

```
cp env.example .env
```

3, Set strong passwords in the security section options of .env file by running the a bash script which is in the cloned repo.

```
./gen-passwords.sh  (It will create all the required secret passwords)
```

4, Create required CONFIG directories

```
mkdir -p ~/.jitsi-meet-cfg/{web/crontabs,web/letsencrypt,transcripts,prosody/config,prosody/prosody-plugins-custom,jicofo,jvb,jigasi,jibri}
```
This is the location where all the configuration are saved.

5, Add domain name in .env, 

```
variable name is PUBLIC_URL=https://meetme.medeva.io
```
6, Copy the folder /usr/share/jitsi-meet from jitsi-web docker container and mount as volume mount in docker-compose.yml. We can edit the files in the folder jitsi-meet for custamizing the jitsi in our way. The volume mount will update the changes without the container restart.
Example code is added below.
```
        image: jitsi/web:stable-6433
        restart: ${RESTART_POLICY}
        ports:
            - '${HTTP_PORT}:80'
            - '${HTTPS_PORT}:443'
        volumes:
            - ${CONFIG}/web:/config:Z
            - ${CONFIG}/web/crontabs:/var/spool/cron/crontabs:Z
            - ${CONFIG}/transcripts:/usr/share/jitsi-meet/transcripts:Z
            - ./jitsi-meet:/usr/share/jitsi-meet
```
7, Finally Run 
```
docker-compose up -d
```

> Update Jitsi

For updating, change the image name in docker-compose.yml(make sure to use stable images)
