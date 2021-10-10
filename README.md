# AEM Sample Site

Sample AEM project to build components, services. Setup dispatcher and understand typical dispatcher configurations.

## Pre-requisite

* JDK 11
* Maven 3.8+
* Docker 20+
* Git 2.32
* Host file has below entries pointing to localhost
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

    docker load -i aem-redesign-dispatcher.tar

Run `docker-compose up -d` to start the dispatcher

## Modules

The main parts of the template are:

* core: Java bundle containing all core functionality like OSGi services, listeners or schedulers, as well as
  component-related Java code such as servlets or request filters.
* dispatcher: Basic dispatcher configurations
* ui.apps: contains the /apps (and /etc) parts of the project, ie JS&CSS clientlibs, components, and templates
* ui.content: contains sample content using the components from the ui.apps
* ui.config: contains runmode specific OSGi configs for the project
* all: a single content package that embeds all of the compiled modules (bundles and content packages) including any
  vendor dependencies

## How to build

To build all the modules run in the project root directory the following command with Maven 3:

    mvn clean install

To build all the modules and deploy the `all` package to a local instance of AEM, run in the project root directory the
following command:

    mvn clean install -PautoInstallSinglePackage

Or to deploy it to a publish instance, run

    mvn clean install -PautoInstallSinglePackagePublish

Or alternatively

    mvn clean install -PautoInstallSinglePackage -Daem.port=4503

Or to deploy only the bundle to the author, run

    mvn clean install -PautoInstallBundle

Or to deploy only a single content package, run in the sub-module directory (i.e `ui.apps`)

    mvn clean install -PautoInstallPackage

## Maven settings

The project comes with the auto-public repository configured. To setup the repository in your Maven settings, refer to:

    http://helpx.adobe.com/experience-manager/kb/SetUpTheAdobeMavenRepository.html
