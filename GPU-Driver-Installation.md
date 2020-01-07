Ant Media Server can use hardware-based encoder that is available in some NVIDIA GPUs. If you have a NVIDIA GPU, you can check that your GPU contains hardware-based encoder on [Video Encode and Decode GPU Support Matrix](https://developer.nvidia.com/video-encode-decode-gpu-support-matrix)

# Why to Use NVIDIA GPU Encoder?

Answer is Performance. Performance increases 5x over x264(CPU) encoder. Btw, x264 is one of the best h.264 software encoder and Ant Media Server uses x264 if there is no GPU in the system.

![](images

# Install CUDA Toolkit

After you are sure that your GPU contains hardware-based encoder, the only thing is installing CUDA toolkit to your system.

# Installation on Ubuntu 16.04 and Ubuntu 18.04

### Ubuntu 16.04

`wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_10.2.89-1_amd64.deb`

Install repository meta-data

`dpkg -i cuda-repo-ubuntu1604_10.2.89-1_amd64.deb`

Import CUDA Public GPG Key

`sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub`

### Ubuntu 18.04

`wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-repo-ubuntu1804_10.2.89-1_amd64.deb`

Import CUDA Public GPG Key

`sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub`

### Ubuntu 16.04 and Ubuntu 18.04

Update repository cache

`sudo apt-get update`

Install CUDA Runtime 10.0

`sudo apt-get install cuda-runtime-10-0`

If you've installed latest version of CUDA runtime(such 10.1) and it does not work, you can install following packets for compatibility
```
sudo apt-get install cuda-cudart-10-0
sudo apt-get install cuda-compat-10-0
```
If everthing is ok, you can run the command below to see the status of your GPU

`nvidia-smi`

You can install Ant Media Server with its usual way or if you already install it, you can restart the Ant Media Server.

`sudo service antmedia restart`

# Using NVIDIA Hardware-based Encoder

Ant Media Server will check and log at startup if there is a hardware-based GPU encoder in the system and it will use it automatically. No need to do anything.

If you need more information for installing on other systems, please check [NVIDIA](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html) docs and [CUDA downloads](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=debnetwork) pages

If you have any questions or comments,  just send an email to contact at antmedia dot io or fill the <a href="https://antmedia.io/#contact">contact form.</a>
