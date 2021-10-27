# AEM Sample Site

Sample AEM project to build components, services. Setup dispatcher and understand typical dispatcher configurations.

## Pre-requisite

* JDK 11+
* Maven 3.8+
* Docker 20+
* Git 2.32+
* Host file has below entries and they point to localhost
    * author.local
    * publish.local

## Generate the project

Create project with maven multi-module archtype version 24. Execute below command with elevated privileges. `appId` will
be the project root directory.

    mvn -B archetype:generate \
    -D archetypeGroupId=com.adobe.aem \
    -D archetypeArtifactId=aem-project-archetype \
    -D archetypeVersion=24 \
    -D appTitle="AEM Sample Site" \
    -D appId="aem-sample-site" \
    -D groupId="com.sy.aem" \
    -D aemVersion="6.5.0"

## Build the docker image

    docker build \
    -t aem-sample-site/dispatcher:latest \
    --build-arg MODULE_URL=https://download.macromedia.com/dispatcher/download/dispatcher-apache2.4-linux-x86_64-4.3.3.tar.gz \
    .

## Create and start container

    docker-compose up -d

Validate author dispatcher changes by accessing http://author.local/ and publish dispatcher changes by
accessing http://publish.local/

## Stop and remove container

    docker-compose down

## Additional guide on saving the docker image and loading the same from other workstations

### List docker images

    docker images

### Save the docker image

Get the docker image id from above list and execute below command

    docker save -o ./aem-sample-site.tar aem-sample-site/dispatcher

### Load the image

Copy the above image to any workstation and load the image

    docker load -i aem-sample-site.tar

Run `docker-compose up -d` to start the dispatcher

Note: Dispatcher setup is referred
from `https://blogs.perficient.com/2021/01/05/setting-up-a-local-aem-dispatcher-with-docker/`
