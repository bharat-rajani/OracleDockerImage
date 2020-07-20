Optimized Oracle XE 18c on Docker
=====
Image size: 3.9GB

Build a Docker image containing Oracle XE 18c with optimized size (3.9 GB instead of 12 GB) 


## Oracle XE 18c Software
[Download Oracle XE 18c](https://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/index.html) `.rpm` file and drop it inside folder `18.4.0`.

## Prerequirements
You will need to complete following prerequirements
```

$ mkdir -p /opt/oradata

```
## Build it
To build the images just run below command:
```
$ ./buildDockerImage.sh -v 18.4.0 -x
. . .
Successfully built ac71812fa9d3
Successfully tagged oracle/database:18.4.0-xe


  Oracle Database Docker Image for 'xe' version 18.4.0 is ready to be extended:

    --> oracle/database:18.4.0-xe

  Build completed in 697 seconds.

$ docker images
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
oracle/database      18.4.0-xe           2018ad58d2f3        16 hours ago        3.9GB

Note: Oracle official db binary may change time to time and due to that checksum might fail. 
You can put updated checksum in Checksum.xe (for express edition) or simply to ignore md5sum check please use -i option.
#For Example:
$ ./buildDockerImage.sh -v 18.4.0 -x -i
```

## Run the Container
Just run following command, which will create your Oracle XE 18c Container, mount the internal directory `/opt/oracle/oradata` to your prior created directories on your Docker host and enables a port forwarding of the Listener Port 1521 to 1521
```
docker run -d --name oraxe18c \
              -p 1521:1521 \
              -e ORACLE_PWD=[your password] \
              -e ORACLE_CHARACTERSET=[your characterset] \
              -e TZ=[your timezone] \
              -v [host directory for oradata]:/opt/oracle/oradata \
              [--network [your bridged network] \
              oracle/database:18.4.0-xe

# For Example
docker run -d --name oraxe18c \
              -p 1521:1521 \
              -e ORACLE_PWD=Oracle18c \
              -e ORACLE_CHARACTERSET=AL32UTF8 \
              -e TZ=Europe/Zurich \
              -v /opt/oradata:/opt/oracle/oradata \
              --network mynet \
              oracle/database:18.4.0-xe
```



## License
To download and run Oracle Database, regardless whether inside or outside a Docker container, you must download the binaries from the Oracle website and accept the license indicated at that page.

All scripts and files hosted in this project and GitHub repository required to build the Docker images are, unless otherwise noted, released under [UPL 1.0](https://oss.oracle.com/licenses/upl/) license.
