# Enabling NVIDIA GPU Encoder Support

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
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_9.2.88-1_amd64.deb
```

Install repository meta-data
```
sudo dpkg -i cuda-repo-ubuntu1604_9.2.88-1_amd64.deb
```

Install CUDA Public GPG Key
```
sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
```

Update repository cache
```
sudo apt-get update
```

Install CUDA
```
sudo apt-get install cuda
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
Ant Media Server will check and log at startup if there is a hardware-based GPU encoder in the system and it will use it automatically. No need to do anything. Log at startup will be as follows pay attention the line `cuda verify result true` below

```
Running on  Linux
Starting Ant Media Server
Root: /usr/local/antmedia
Configuation root: /usr/local/antmedia/conf
Ant Media Server jar was found
URL list: [file:/usr/local/antmedia/ant-media-server.jar]
cuda verify result true
Selected libraries: (175 items)
...
```

If you need more information for installing on other systems, please check [NVIDIA docs](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html) and [CUDA downloads](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=debnetwork) pages
