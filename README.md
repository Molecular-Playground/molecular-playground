# Molecular Playground

### Getting Started
#### Docker
To get started, you first need to install docker. Instructions for installing docker can be found [here](https://docs.docker.com/engine/installation/). After you have docker installed, you will need to install docker compose. Follow the instructions [here](https://docs.docker.com/compose/install/).

#### Clone Recursively
This repo uses [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules). In order to clone everything correctly, you have to clone recursively:
```
git clone --recursive https://github.com/molecular-playground/molecular-playground.git
cd molecular-playground
```
More useful commands for git submodules can be found in [Repository Maintenence](#Repository Maintenence) below.

#### Configuration Files
Configuration files with secure information such as passwords must be created before running the system. Currently there is only one configuration file that must be created, the configuration file for [ms-email](https://github.com/molecular-playground/ms-email#important-setup-instructions).

### To Run
After your environment is all set up, simply run the following commands:
```
docker-compose build
docker-compose up
```
The gateway will be exposed on port 8000!

### Repository Maintenence
For those of you who are not using a GUI to interact with git, here are some useful commands:
```
# after checking out a commit (for example git pull master), this updates your submodules
git submodule update --recursive

# these commands will pull from master for each submodule
git fetch --recursive-submodules
git submodule foreach git pull origin master
git pull
```

### Resetting the Database
The database uses a docker volume to persist data between containers and images. It will first look for and try to use a volume named ```postgres-data```, however if one does not exist it will create a volume named ```molecularplaygorund_postgres-data``` instead. To reset the database, all one has to do is remove the docker volume and run docker-compose again. Here is a sample walkthrough:
```
# remove any containers that are using the volume
# if you used the volume with any other containers you will have to kill them manually
docker-compose rm

# remove the volume
# remember that based on your setup it could be named postgres-data
docker volume rm molecularplayground_postgres-data

# run docker-compose again to create the new volume
docker-compose build
docker-compose up
```

### Troubleshooting
###### "Help! docker-compose says files are missing!"
9 times out of 10 this is due to not updating your submodules when changing commits. Assuming you did a recursive clone of this repo, just run:
```
git submodule update --recursive
```

###### "Help! docker-compose fails at npm install!"
You most likely are running docker on Mac or Windows and have recently switched internet connections. When you switch internet connections while your docker-machine is still running, it looses it's connection. Assuming your docker-machine is called default, run:
```
docker-machine restart default
```
