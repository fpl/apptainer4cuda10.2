# Apptainer for Cuda Toolkit 10.2

This is the Apptainer definition file to be used in order to use any card that
is not supported by the current Cuda Toolkit (12.x at the date of this code).
Note that it is based on the pass-through feature for Nvidia to use the card
within a container, which is NOT supported for all cards.
 
 In my case, this is working with a Quadro K5000 (Kepler arch) on my refurbished
workstation, a card dated  2014, which is supported only up to toolkit version
10.2. Note that, but for that, the toolkit requires a GCC 8.x, which is only
available until Debian 10 (Buster).
 
This is the reason why this container is based on `oldoldstable`, which is not
more regularly updated in Debian archives.

Basically, this is also the standard way to use any modern HPC-GPU cluster to
run GPU-enabled code.

[Here](https://developer.nvidia.com/cuda-10.2-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=runfilelocal)
the Cuda Toolkit is still available at the date of this repository.

```
mkdir $HOME/cuda && cd $HOME/cuda
wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda_10.2.89_440.33.01_linux.run
wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/patches/1/cuda_10.2.1_linux.run
wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/patches/2/cuda_10.2.2_linux.run
```

In order to use Cuda code properly, run

```
apptainer build cuda10.2.sif cuda10.2.def
apptainer shell --nv cuda10.2.sif
```

Eventually, it is possible to create derived SIF images starting from the
basic generated image with the toolkit in order to use other tools.

If all is working correctly you should get something like:

```
$ apptainer exec --nv cuda10.2.sif nvidia-smi

Tue Feb 25 17:57:40 2025       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 470.199.02   Driver Version: 470.199.02   CUDA Version: 11.4     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  Quadro K5000        Off  | 00000000:03:00.0  On |                  Off |
| 33%   51C    P0    41W / 137W |   1003MiB /  4035MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A      4146      G   /usr/lib/xorg/Xorg                312MiB |
|    0   N/A  N/A      4555      G   /usr/bin/gnome-shell              184MiB |
|    0   N/A  N/A      8860      G   /opt/firefox/firefox              455MiB |
+-----------------------------------------------------------------------------+

```
