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
For those of you who are not using a superior GUI to interact with git, here are some useful commands:
```
# after checking out a commit (for example git pull master), this updates your submodules
git submodule update --recursive

# these commands will pull from master for each submodule
git fetch --recursive-submodules
git submodule foreach git pull origin master
git pull
```
