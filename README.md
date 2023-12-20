# Build a custom airflow image for production
Although there's a lot of off-the-shelf airflow images on dockerhub, they are made for convience instead of production.
To bring your workload to a production, your need to make sure the vulnerability is under control.
This is where building your Airflow image from scratch comes into play."

## Getting started
This repository offers a Dockerfile accompanied by a Docker Compose YAML file. These resources are provided for those who wish to host Airflow on their local computer or a standalone machine.

## Version of Airflow and Python we're using
* python==3.8
* airflow==2.8.0

The version we have opted to build is determined based on the existing vulnerabilities(no high and critical level).

**note**: feel free to change the version of Python in Dockerfile, if you need higher version.

## Other packages
Here's a sample list of packages for running Airflow:
you can add your own packages to this requirements.
```t
apache-airflow-providers-celery>=3.4.1 ; python_version >= "3.8" 
redis==4.6.0
apache-airflow==2.8.0 ; python_version >= "3.8"

```
### Build docker image
Two ways to build you own Airflow image:
* Build from Dockerfile in the repository
    1. clone the repository
    ```
    git clone git@github.com:hero710690/airflow_custom_built_docker.git
    ```
    2. build
    ```shell
    docker build -t airflow-custom:2.8.0 .
    ```
    or build with specified dockerfile
     ```shell
    docker build -t airflow-custom:2.8.0_py3.10 -f Dockerfile.py310
    ```
* Create you own Dockerfile using the image on DockerHub as the base image
    ```Dockerfile
    FROM jeanlee/airflow-custom:2.8.0
    ...
    ```
### Start the Airflow Services
* Build docker network for the network between Airflow services
    ```shell
    docker network create airflow_network
    ```
* Utilize the Dockerfile in the repository to build and initiate the service.
    #### check the yaml configuration
    ```yaml
    version: '3'
    x-airflow-common:
    &airflow-common
    # leave the following line commented out
    #image: ${AIRFLOW_IMAGE_NAME:-jeanlee/airflow-custom:2.8.0}
    build: .
    ```
    #### execute docker compose command
    ```shell
    docker-compose up -d --build
    # Run the container in the background
    ```
    This command will start the Airflow services, building the necessary components as specified in the provided Dockerfile. The -d flag ensures the container runs in the background for seamless execution.

* Run the service without building image
    ```yaml
    version: '3'
    x-airflow-common:
    &airflow-common
    # replcae the image field with your own image
    image: ${AIRFLOW_IMAGE_NAME:-jeanlee/airflow-custom:2.8.0b1}
    # leave the following line commented out
    # build: .
    ```
    #### execute docker compose command
    ```shell
    docker-compose up -d
    # Run the container in the background
    ```