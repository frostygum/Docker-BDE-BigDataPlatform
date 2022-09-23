# Docker-BDE-BigDataPlatform

**Tested OK using:**
- Docker v14.12.0 on Windows 11
- Docker v14.12.0 on Mac OS 

Currently this repository provide following services:
- namenode (Hadoop HDFS)
- datanode (Hadoop HDFS)
- resourcemanager (Hadoop YARN)
- nodemanager (Hadoop YARN)
- historyserver (Hadoop History Server)
- zookeeper
- hbase-master (Hbase)
- hbase-region (HBase)
- hive-server (Hive)
- hive-metastore (Hive)
- hive-metastore-postgresql (Hive)

Namenode service has mapping volume from local folder called **data** so please makesure that folder is exists before running the installation processes.
The **data** folder is maps any file and changes from your local machine to docker namenode container and vise versa.
So that you can put any data types or folder you want to your HDFS trough this folder.


## Installation

- Download or clone this repository
    ```bash
    git clone https://github.com/frostygum/Docker-BDE-BigDataPlatform big-data
    ```
- Move working diretory
    ```bash
    cd big-data
    ```
- Build images and pull from docker repository (online and require internet connection)
    ```bash
    docker-compose build
    ```
- Starting Services:
    - All Services:
        ```bash
        docker-compose up -d
        ```
    - Selected Services **(i.e. namenode and datanode only)**:
        ```bash
        docker-compose up -d namenode datanode
        ```
        You can also added more services that matches name of services in *docker-compose.yaml* file.
- Stopping Services:
    ```bash
    docker-compose stop
    ```

## Documentation

### Hadoop HDFS Only
Minimum Hadoop HDFS requires at least following services:
- namenode (https://localhost:9870)
- datanode

namenode will be the master that controlls datanode. the amount of datanode can be more than 1. 
The default file in this repo only have 1 datanode, but you can added more by copying the datanode configuration in docker-compose.yaml and rename it.
The services will be:
- namenode
- datanode1
- datanode2
- datanodeN

If you need to put 1 or more data:
- copy file(s) or folder(s) to data folder.
- open namenode service container from terminal
    ```bash
    docker-compose exec namenode bash
    ```
- makesure file(s) or folder(s) in /data folder in container matches with folder data in this repo in yout local machine.
- copy file from container to HDFS
    ```bash
    hdfs dfs -put /pathInContainer/test.txt /pathInHDFS/test.txt
    ```
    If you don't have folder /pathInHDFS you need to create it manually, then run again above command
    ```bash
    hdfs dfs -mkdir /pathInHDFS
    ```

### Hadoop YARN Only
Minimum Hadoop YARN requires at least following services:
- resourcemanager (https://localhost:8088)
- nodemanager

resourcemanager will be the master that controlls nodemanager. the amount of nodemanager can be more than 1. 
The default file in this repo only have 1 nodemanager, but you can added more by copying the datanode configuration in docker-compose.yaml and rename it.
The services will be:
- namenode
- nodemanager1
- nodemanager2
- nodemanagerN

### Hadoop Completed
Minimum Complete Hadoop requires at least following services:
- namenode (https://localhost:9870)
- datanode
- resourcemanager (https://localhost:8088)
- nodemanager
- historyserver (optional) (https://localhost:8188)

The datanode and nodemanager also can be more than 1, you can refrence it from above configuration.
The historyserver on Hadoop is optional, it is "log of activity" that YARN has been executed.

### Hive and HDFS
Minimum Hive requires at least following services:
- namenode (https://localhost:9870)
- datanode
- hive-server
- hive-metastore
- hive-metastore-postgresql

If you need to run SQL commands on hive:
- open hive-server service container from terminal
    ```bash
    docker-compose exec hive-server bash
    ```
- open hive terminal on container
    ```bash
    /opt/hive/bin/beeline -u jdbc:hive2://localhost:10000
    ```
- make sure it is outputed "connected ..." on terminal

Testing the connection:
- Create table named pokes
    ```bash
    CREATE TABLE pokes (foo INT, bar STRING);
    ```
- Show all table on hive
    ```bash
    SHOW TABLES;
    ```
    It should has output a table pokes that we have been created before.

### HBase and HDFS
Minimum HBase requires at least following services:
- namenode (https://localhost:9870)
- datanode
- resourcemanager (https://localhost:8088)
- nodemanager
- zookeeper
- hbase-master (https://localhost:16010)
- hbase-region

You could check if HBase running correctly by open http://localhost:16010 in your prefered web browser.
The HBase page should show at least 1 region server, and don't have any error messages.
