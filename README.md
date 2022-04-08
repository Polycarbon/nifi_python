# Running zookeeper container on cluster 
pull zookeeper image

    sudo docker pull bitnami/zookeeper:latest

setup zookeeper.env on each host

###Host 1
    ZOO_SERVER_ID=1
    ALLOW_ANONYMOUS_LOGIN=yes
    ZOO_SERVERS=data-node1:2888:3888,data-node2:2888:3888,data-node3:2888:3888

###Host 2
    ZOO_SERVER_ID=2
    ALLOW_ANONYMOUS_LOGIN=yes
    ZOO_SERVERS=data-node1:2888:3888,data-node2:2888:3888,data-node3:2888:3888

###Host 3
    ZOO_SERVER_ID=3
    ALLOW_ANONYMOUS_LOGIN=yes
    ZOO_SERVERS=data-node1:2888:3888,data-node2:2888:3888,data-node3:2888:3888

start zookeeper container on all hosts in cluster
    
    `sudo docker run --name zookeeper --env-file zookeeper.env --network="host" bitnami/zookeeper:latest`

# Running nifi container on cluster

## Building
The Docker image can be built using the following command:

    docker build -t nifi:latest .

This build will result in an image tagged apache/nifi:latest

    # user @ puter in ~/Development/code/apache/nifi/nifi-docker/dockerhub
    $ docker images
    REPOSITORY               TAG                 IMAGE ID            CREATED                 SIZE
    nifi                    latest              f0f564eed149        A long, long time ago   1.62GB

## Running a container

### Cluster instance
Run a NiFi instance to all host is as follows:

    sudo docker run --name nifi-node --env-file nifi.env --network="host" \
    -v /opt/jdbc_drive:/opt/jdbc_drive \
    -v /home/dataadm/ga_scripts:/opt/nifi-scripts \
    -v /opt/dataflow/nifi-1.16.0/content_repository:/opt/nifi/nifi-current/content_repository \
    -v /opt/dataflow/nifi-1.16.0/database_repository:/opt/nifi/nifi-current/database_repository \
    -v /opt/dataflow/nifi-1.16.0/flowfile_repository:/opt/nifi/nifi-current/flowfile_repository \
    -v /opt/dataflow/nifi-1.16.0/provenance_repository:/opt/nifi/nifi-current/provenance_repository \
    -v /opt/dataflow/nifi-1.16.0/logs:/opt/nifi/nifi-current/logs \
    -v /opt/dataflow/nifi-1.16.0/state:/opt/nifi/nifi-current/state \
    nifi:1.16.0

This will provide a running instance, exposing the instance UI to the host system on at port 8443,
viewable at `https://localhost:8443/nifi`.
