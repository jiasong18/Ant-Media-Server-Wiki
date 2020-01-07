# Using NVIDIA Hardware-based Encoder on Docker

On host(Ubuntu 16.04 or Ubuntu 18.04)

* Install docker-ce according to the link - https://docs.docker.com/install/
```
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```
* Install nvidia-docker2
```
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
```
## Install nvidia-docker2 and reload the Docker daemon configuration
* Ubuntu 16.04 and Ubuntu 18.04
```
sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd
```
Start a docker container with following command
* Ubuntu 16.04
```
sudo docker run --runtime=nvidia \
 --privileged --network host --name cuda-docker2 \
 -e NVIDIA_VISIBLE_DEVICES=all -e NVIDIA_DRIVER_CAPABILITIES=compute,utility,video \
 -it nvidia/cuda:10.0-runtime-ubuntu16.04
```
* Ubuntu 18.04
```
sudo docker run --runtime=nvidia \
 --privileged --network host --name cuda-docker2 \
 -e NVIDIA_VISIBLE_DEVICES=all -e NVIDIA_DRIVER_CAPABILITIES=compute,utility,video \
 -it nvidia/cuda:10.0-runtime-ubuntu18.04
```
In this docker container, you can install Ant-Media-Server Enterprise edition. It automatically uses hardware encoder

If you have any questions or comments,  just send an email to contact at antmedia dot io or fill the <a href="https://antmedia.io/#contact">contact form.</a>
