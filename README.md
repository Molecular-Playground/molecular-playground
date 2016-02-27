# Molecular Playground

### Getting started
To get started, you first need ot install docker. Instructions for install docker can be found [here](https://docs.docker.com/engine/installation/). After you have docker installed, you will need to install docker compose. Following the instructions [here](https://docs.docker.com/compose/install/).

### To Run
After installation is all set up, simply run ```docker-compose up```.

### Resetting the Database
To reset the database you will need to remove the postgres container as well as the data volume for the database. First check if you have a postgres container running ```docker ps```. If you have a postgres container running, run ```docker kill molecularplayground_postgres_1``` to kill the container and then ```docker rm molecularplayground_postgres_1``` to remove the container. The last step is to remove the data volume, run ```docker volume rm molecularplayground_postgres-data```

### To access the API
Send all requests to port 8000.