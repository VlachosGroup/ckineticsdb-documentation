# CKineticsDB

The Chemical Kinetics Database (CKineticsDB) provides simulation data associated with multiscale modeling in heterogeneous catalysis. It provides data at three scales: ab-initio calculations, thermochemistry, and microkinetic models.

<img src="https://github.com/VlachosGroup/ckineticsdb-documentation/blob/main/images/overview.PNG?raw=true"/>

CKineticsDB stores the unaltered, curated simulation files from the multiscale modeling workflow at three scales: (1) electronic structure calculations (DFT) input and output; (2) statistical mechanics input and thermochemistry output; and (3) microkinetic modeling (MKM) input data.

**Getting Started:**
1.	Install Docker as per the instructions given on the [Docker website](https://docs.docker.com/get-docker/). 
2. 	Download and start the Docker-based MongoDB database as per [instructions](https://github.com/VlachosGroup/ckineticsdb-documentation/blob/main/Docker_Mongo.md).
3. Download and launch the CKineticsDB desktop application as per the [instructions](https://github.com/VlachosGroup/ckineticsdb-documentation/blob/main/CKineticsDB_Application.md).

CKineticsDB is a part of the Virtual Kinetics Laboratory (VLab) Software Ecosystem developed by Vlachos Group at the University of Delaware as a part of the RAPID Reaction Software Ecosystem. Information on other tools in VLab can be found at the [Delaware Energy Institute's RAPID Site](https://dei.udel.edu/rapid/rapid-research/rapid-reaction-software-ecosystem/)

### Citing this work
Please use the paper at: https://pubs.acs.org/doi/10.1021/acs.jcim.3c00123 


## CKineticsDB components for download

### A.	Data

<ins>Download from</ins>: https://files.ccei.udel.edu/p/CKineticsDB/data/ 

The above link contains the following data dumps generated from MongoDB using mongodump

Contents of the ‘latest’ directory:
- ckineticsdb-demo.data.gz: contains one dataset to try CKineticsDB software before downloading the large complete data dump
- ckineticsdb-all.data.gz: contains the entire CKineticsDB datahub data  

Old versions of the data dump are also available based on the date they were archived.

### B.	Software

Latest release (12-15-2023): v1.0.1. 

<ins>Download from:</ins> https://files.ccei.udel.edu/p/CKineticsDB/sw/ 

Accessing the software components requires user credentials. These can be obtained by reaching out to vkineticslab@udel.edu .

The above link contains the following software components:
- Docker container which runs a MongoDB server. It automatically downloads into the container the ckineticsdb-demo.data.gz data dump by default. This can be overridden to download other datasets in the  https://files.ccei.udel.edu/p/CKineticsDB/data/ repository or plug in an already downloaded locally available data dump from CKineticsDB.
- Desktop application to be used either as a graphical user interface or a command line interface. The application automatically connects to the MongoDB server in the shared Docker container.
- Necessary files and templates to use CKineticsDB.
