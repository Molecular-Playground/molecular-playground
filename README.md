# Molecular Playground

### Getting started
To get started, you first need to install docker. Instructions for installing docker can be found [here](https://docs.docker.com/engine/installation/). After you have docker installed, you will need to install docker compose. Follow the instructions [here](https://docs.docker.com/compose/install/).

### To Run
After installation is all set up, simply run ```docker-compose up```

### Resetting the Database
To reset the database you will need to remove the postgres container as well as the data volume for the database.
```
docker kill molecularplayground_postgres_1
docker rm molecularplayground_postgres_1
docker volume rm molecularplayground_postgres-data
docker-compose up
```

### To access the API
Send all requests to port 8000.

### Repository Maintenence
Given that this repository uses sub-modules for it's composition, we have included a script to reduce the possibilty of keeping an outdated copy of a submodule in the repository. Whenever updating source code through the command line, use the command:
```
bash anti-medusa.sh
```
from the base folder of this repository. This script will recursively update all submodules in the repository.
Please note that this is for the command line implementation. A GUI is recommended for further errors.
