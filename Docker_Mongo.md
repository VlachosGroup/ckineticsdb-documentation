# Running the MongoDB datahub

1. **Docker Image**:
Download the Docker image ‘ckineticsdb-database.tar’ from the software download path specified on the main page


2.	**Load Image**:

    •	Launch Docker Desktop and ensure that it is running (this can take a few seconds up to a minute)

    •	Open the shell / command prompt and run:


```python
docker load --input ../ckineticsdb-database.tar
```

Use the appropriate path to the ckineticsdb-database.tar file.

Check if the image (ckineticsdb-database:latest) is seen in Docker Desktop --> Images

3.	**Running the Container for the first time**:

When running the container for the first time, the data to be injected into MongoDB can be specified as per location of the data:
    
    3.1. Inject the default (latest) sample data dump available on https://files.ccei.udel.edu/p/CKineticsDB/data/ 
    3.2. Inject a desired data dump available on https://files.ccei.udel.edu/p/CKineticsDB/data/
    3.3. Inject a data dump available locally on the host machine
    
Open the shell / command prompt and run the respective commands to inject the desired data dump. These are explained below in detail for each of the above mentioned data locations.

<ins>Note</ins>: We recommend testing the entire workflow (software application and docker-Mongo database) and software features using the 'ckineticsdb-demo.data.gz' data first. It is a much smaller data dump so it downloads and loads into Docker fast. This will help to ensure that all the components are properly installed and working.

## Inject the default (latest) sample data dump 

    Dataset name: ckineticsdb-demo.data.gz
    Available on https://files.ccei.udel.edu/p/CKineticsDB/data/ 


```python
docker run --name ckineticsdb-db -p 27017:27017 ckineticsdb-database:latest
```

   Running this will download the default (latest) sample dataset and restore it in the MongoDB database inside the container.

   Explanation:
   
       --name ckineticsdb-db: specifies name of the container
       -p 27017:27107: Publishes port 27017 of the container and binds it to the port 27017 of the host

## Inject a desired data dump

    For example, Dataset name: ckineticsdb-all.data.gz
    Available on https://files.ccei.udel.edu/p/CKineticsDB/data/


```python
docker run -e CKINETICSDB_ARCHIVE_URL=https://files.ccei.udel.edu/p/CKineticsDB/data/latest/ckineticsdb-all.data.gz --name ckineticsdb-db -p 27017:27017 ckineticsdb-database:latest
```

Running this will download the specified dataset at the URL and restore it in the MongoDB database inside the container.

Explanation:

    --name ckineticsdb-db: specifies name of the container
    -p 27017:27107: Publishes port 27017 of the container and binds it to the port 27017 of the host
    -e CKINETICSDB_ARCHIVE_URL=<URL of desired dataset>: updates the default value of the environment variable inside the docker container with the specific URL

## Inject a data dump available locally on the host machine

This data dump can be either a manually downloaded data dump from the provided data download URL or a back-up created from the Docker-based MongoDB database provided by CKineticsDB.

    For example, dataset name: ckineticsdb-old.data.gz


```python
docker run --name ckineticsdb-db -v ../Desktop/backup:/tmp/backup -e CKINETICSDB_ARCHIVE=/tmp/backup/ckineticsdb-old.data.gz -p 27017:27017 ckinetics-database:latest
```

Running this will use the specified dataset from the local user’s machine and restore it in the MongoDB database inside the container.


Explanation:

    --name ckineticsdb-db: specifies name of the container
    -p 27017:27107: Publishes port 27017 of the container and binds it to the port 27017 of the host

In this case, a local directory needs to be attached to the docker container. This directory should contain the data dump to be injected into MongoDB inside the container. This is achieved with the flag “-v ../Desktop/backup:/tmp/backup” in the above command. Here, the local directory ‘Desktop/backup’ contains the data dump ‘ckineticsdb-old.data.gz’. This directory is mapped to the directory “/tmp/backup” inside the docker container. 

Now, the path to the data dump inside the Docker container needs to be specified using an environment variable used in the command above as “CKINETICSDB_ARCHIVE=/tmp/backup/ckineticsdb-old.data.gz” and explained below:

-e CKINETICSDB_ARCHIVE=<path of data dump inside the Docker container>: updates the default value of the environment variable inside the docker container with the specific path

## Starting the container after the first run

When the container is run the first time, the specified data is downloaded and injected into the MongoDB database inside the container and stays persistent as long as the container is not deleted. Even if Docker is not running, the container and the data inside it will stay persistent. Thus, the data download and injection only happens when the container is run for the first time. This first run can take upto ~20 minutes to upload the ckineticsdb-all.data.gz data dump. More data can be added to the datahub using the data upload features of CKineticsDB and the container can be used as a local database for managing users’ data locally. 

To restart the container, follow the following steps:

Ensure that Docker Desktop is running. 

Open shell / command prompt and run the command:


```python
docker start CONTAINER
```

where, CONTAINER is the name of the container which needs to be started.

Alternatively, users can also use the Docker Desktop Application to start a container as explained below:
Open Docker Desktop; Navigate to ‘Containers’; Select the Container name; Click on the start icon at the top.

<img src="https://github.com/siddhantlambor/ckineticsdb-documentation/blob/main/images/docker%20container%20start.PNG"/>

# Backup of the Uploaded Data

A feature of CKineticsDB is that users can upload their data to the datahub and use CKineticsDB as a data management tool locally. This data will be saved in the Docker container. Under unforeseen circumstances, if this container is deleted, the stored data cannot be recovered. Thus, it is recommended for users to take regular backup of the data inside the container and store it on the host machine outside the container. 

Create a data dump of the data stored in the database:

- Start Docker Desktop
- Start the container as explained in the previous section
- Open a command prompt / shell and run the following command to open the bash shell inside the container:



```python
docker exec -it CONTAINER bash
```

where, CONTAINER is the name of the container to launch a bash shell in 

- This should launch a bash shell. Inside the bash shell, run the following command to export the database’s contents into a binary file.



```python
mongodump -u USERNAME -p PASSWORD --db=ckineticsdb --archive=/tmp/backup.data.gz --gzip  mongodb://localhost/ckineticsdb
```

where,
    
    USERNAME = username provided with the database credentials
    PASSWORD = password provided with the database credentials
    --archive=/tmp/backup.data.gz: specified the location and name of the file in which the database contents are stored
    --gzip: compresses the data dump 
    mongodb://localhost/ckineticsdb: connection string for the MongoDB instance


- The above command will create a file named ‘backup.data.gz’ in the directory /tmp/ inside the docker container.

**To copy the data dump file from the container to the host:**

- Open a shell / command prompt and run the command:


```python
docker ps -a
```

This will display the details of all the existing containers. Note down the CONTAINER-ID or the container in which the data dump has been created.

- Run the following command to copy the data dump to the host:



```python
docker cp CONTAINER-ID:SRC DEST
```

where,
SRC is the path of the data dump file inside the container,
DEST is the destination on the host where the file should be copied.

This data dump can be injected into a fresh Docker image provided by CKineticsDB by following the instructions shown in section 3.3. above.
