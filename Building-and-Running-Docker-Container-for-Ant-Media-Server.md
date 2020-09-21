* First of all we create a text file with name of `Dockerfile` as follows:
```
# This docker file can be used in kubernetes. 
# It accepts all cluster related parameters at run time. 
# It means it's very easy to add new containers to the cluster 

FROM ubuntu:18.04

ARG AntMediaServer

# Keep this value ARGs for compatibility
ARG MongoDBServer=
ARG MongoDBUsername=
ARG MongoDBPassword=

#Running update and install makes the builder not to use cache which resolves some updates
RUN apt-get update && apt-get install -y curl libcap2 wget net-tools 

ADD ./${AntMediaServer} /home

RUN cd home \
    && pwd \
    && wget https://raw.githubusercontent.com/ant-media/Scripts/master/install_ant-media-server.sh \
    && chmod 755 install_ant-media-server.sh

RUN cd home \
    && pwd \
    && ./install_ant-media-server.sh -i ${AntMediaServer} -s false

RUN update-java-alternatives -s java-1.8.0-openjdk-amd64

# Keep this for compatibility
RUN /bin/bash -c 'if [ ! -z "${MongoDBServer}" ]; then \
                    /usr/local/antmedia/change_server_mode.sh cluster ${MongoDBServer} ${MongoDBUsername} ${MongoDBPassword}; \
                 fi'

ENTRYPOINT service antmedia restart && bash

# Options 
# -g: Use global(Public) IP in network communication. Its value can be true or false. Default value is false.
#
# -s: Use Public IP as server name. Its value can be true or false. Default value is false.
#
# -r: Replace candidate address with server name. Its value can be true or false. Default value is false
#
# -m: Server mode. It can be standalone or cluster. Its default value is standalone. If cluster mode is 
#     specified then mongodb host, username and password should also be provided
#
# -h: MongoDB host
#
# -u: MongoDB username
#
# -p: MongoDB password

ENTRYPOINT ["/usr/local/antmedia/start.sh"]
```

* Download or save `<Ant Media Server installation zip>` file in the same directory with `Dockerfile`. Then run the docker build command.

`docker build --network=host -t antmediaserver --build-arg AntMediaServer=<Replace_With_Ant_Media_Server_Zip_File> .` 

* Now we have a docker container with Ant Media Server. Let's run it.

`docker run --network=host --privileged=true -it antmediaserver`