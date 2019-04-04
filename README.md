# CI Tools Demo

This repository contains Dockerfiles for running a set of Continuous Integration Tools with a single command. The diagram contains all tools used in the Docker containers.

![Docker CI Tools](resources/images/ci-tools.jpg)

## Requirements

You should have a Ubuntu Server and Git installed

### Step 0 - Install Pre-requisites.

In order to install the corresponding pre-requisites run the following command:

```bash
./pre-requisites.sh
```

## Getting started

To get all docker containers up and running use:

```bash
./setup.sh
```

## Access Tools

| *Tool* | *Link* | *Credentials* |
| ------------- | ------------- | ------------- |
| Jenkins | 'http://${Server IP Address}:18080/' | no login required |
| SonarQube | 'http://${Server IP Address}:19000/' | admin/admin |

## Extras

Run the following command to clean docker assets.

```bash
./clean.sh
```

<!-- TODO: Add Jenkins jobs description -->


## Deployment process to staging
### Machines
 - http://ci.wocs3.com:18080 Jenkins CI server [162.13.157.169]
 - https://staging.mobility.wocs3.com Staging server [134.213.210.138]

### Steps
Developer makes/merge commit/s to develop branch and Jenkins starts the following jobs:
  - `mbobject_metadata_1-unittests`: Build a dev-(build_number) Docker image (into the Jenkins CI master server local repository)
  - `mbobject_metadata_2-sonar`: Clone current project and run Sonar.
  - `mbobject_metadata_3-docker`: Publish Docker image to Docker Hub.
  - `mobility_staging_deployment`: Clones mb_deployment and edit docker-compose.yml specifying the new dev image and reloads containers.
  - Server is ready at: https://staging.mobility.wocs3.com/mb/
