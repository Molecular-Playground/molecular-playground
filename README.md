# Molecular Playground

### Getting Started
##### Docker
To get started, you first need to install docker. Instructions for installing docker can be found [here](https://docs.docker.com/engine/installation/). After you have docker installed, you will need to install docker compose. Follow the instructions [here](https://docs.docker.com/compose/install/).

##### Clone Recursively
This repo uses [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules). In order to clone everything correctly, you have to clone recursively:
```
git clone --recursive https://github.com/molecular-playground/molecular-playground.git
cd molecular-playground
```
More useful commands for git submodules can be found in [Repository Maintenence](#repository-maintenence) below.

##### Configuration Files
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

# if you checked out a commit and it added a new submodule, run this command before doing anything else
# it initializes the submodule so commands like the above one will work
git submodule init

# these commands will pull from master for each submodule
git fetch --recurse-submodules
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

###### "Help! I tried updating my submodules recursively but the submodule folder(s) are empty!"
When a new submodule is added to the system and you would like to pull it onto your machine, you must first init the submodule before you are able to update it.
```
git submodule init
git submodule update --recursive
```

###### "Help! I get a weird ```FATAL: could not map anonymous shared memory: Cannot allocate memory``` error when I run docker-compose up!"
This error occurs when the postgres container runs out of memory during docker-compose. While the suggested solution is to simply use a machine with more memory, it may be possible to get it up and running with a little bit of work. Using the following commands, run the postgres container manually to initialize the database. Afterwards you should be able to run docker-compose up without any problems. You will have to do this every time you reinitialize the database by removing the docker volume it uses.
```
# make sure to remove all containers that depend on the molecularplayground_postgres-data volume
# for most cases that means just removing the docker-compose volumes
docker-compose stop
docker-compose rm

# remove the molecularplayground_postgres-data volume
docker volume rm molecularplayground_postgres-data

# create a postgres container as described in the databaes submodule with the volume name molecularplayground_postgres-data
cd databaes
docker build -t postgres .
docker run -d --name postgres -e POSTGRES_PASSWORD=dankmemes -v molecularplayground_postgres-data:/var/lib/postgresql/data postgres
cd ..

# remove said postgres container
docker stop postgres
docker rm postgres

# you should be all set!
docker-compose build
docker-compose up -d
```
