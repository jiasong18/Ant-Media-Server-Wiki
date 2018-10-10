* First of all we create a text file with name of `Dockerfile` as follows:
```
FROM ubuntu:16.04

ARG AntMediaServer

RUN apt-get update 
RUN apt-get install -y libx11-dev
RUN apt-get install -y wget

ADD ./${AntMediaServer} /home

RUN cd home \
    && pwd \
    && wget https://raw.githubusercontent.com/ant-media/Scripts/master/install_ant-media-server.sh \
    && chmod 755 install_ant-media-server.sh

RUN cd home \
    && pwd \
    && ./install_ant-media-server.sh ${AntMediaServer}

ENTRYPOINT service antmedia restart && bash
```

* Download or save `<Ant Media Server installation zip>` file in the same directory with Dockerfile. Then run the docker build command.

`docker build --network=host -t <container_name> --build-arg AntMediaServer=<Ant Media Server installation zip> .`

* Now we have a docker container with Ant Media Server. Lets run it.

`docker run --privileged=true -it <container_name>`