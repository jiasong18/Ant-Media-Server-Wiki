***
**_NOTE:_** We have updated our documentation. This page is outdated. You can access updated version from the sidebar menu.
***
Ant Media Server can use hardware-based encoder that is available in some NVIDIA GPUs. If you have a NVIDIA GPU,
you can check that your GPU contains hardware-based encoder on [Video Encode and Decode GPU Support Matrix](https://developer.nvidia.com/video-encode-decode-gpu-support-matrix) 

## Why to Use NVIDIA GPU Encoder?
Answer is Performance. Performance increases 5x over x264(CPU) encoder. Btw, x264 is one of the best h.264 software encoder and Ant Media Server uses x264 if there is no GPU in the system.    

![GPU performance over CPU](https://developer.nvidia.com/sites/default/files/akamai/designworks/images/VidEncode_001_b.png)

## Install CUDA Toolkit
After you are sure that your GPU contains hardware-based encoder, the only thing is installing CUDA toolkit to your system. 

### Installation on Ubuntu 16.04 x86_64

Download the deb file
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_10.1.105-1_amd64.deb
```

Install repository meta-data
```
sudo dpkg -i cuda-repo-ubuntu1604_10.1.105-1_amd64.deb
```

Install CUDA Public GPG Key
```
sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
```

### Installation on Ubuntu 18.04 x86_64

Download the deb file
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1804_10.1.105-1_amd64.deb
```

Install repository meta-data
```
sudo dpkg -i cuda-repo-ubuntu1804_10.1.105-1_amd64.deb
```

Install CUDA Public GPG Key
```
sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
```
### Continue For Ubuntu 16.04 & 18.04 x86_64

Update repository cache
```
sudo apt-get update
```

Install CUDA Runtime 10.0
```
sudo apt-get install cuda-runtime-10-0
```

If you've installed latest version of CUDA runtime(such 10.1) and it does not work, you can install following packets for compatibility
```
sudo apt install cuda-cudart-10-0
sudo apt install cuda-compat-10-0 
```

If everthing is ok, you can run the command below to see the status of your GPU
```
nvidia-smi
```

You can install Ant Media Server with its usual way or if you already install it, you can restart the Ant Media Server.
```
sudo service antmedia restart
```

## Using NVIDIA Hardware-based Encoder
Ant Media Server will check and log at startup if there is a hardware-based GPU encoder in the system and it will use it automatically. No need to do anything.

## Using NVIDIA Hardware-based Encoder on Docker

On host(Ubuntu 16.04) 

* Install docker-ce according to the link - https://docs.docker.com/install/
* Install nvidia-docker2
```
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update

# Install nvidia-docker2 and reload the Docker daemon configuration
sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd
```

* Start a docker container with following command
```
sudo docker run --runtime=nvidia \
 --privileged --network host --name cuda-docker2 \
 -e NVIDIA_VISIBLE_DEVICES=all -e NVIDIA_DRIVER_CAPABILITIES=compute,utility,video \
 -it nvidia/cuda:10.0-runtime-ubuntu16.04
```
In this docker container, you can install Ant-Media-Server Enterprise edition. It automatically uses hardware encoder

 
If you need more information for installing on other systems, please check [NVIDIA docs](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html) and [CUDA downloads](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=debnetwork) pages
