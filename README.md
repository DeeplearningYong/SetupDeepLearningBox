# How to Setup Deep Learning Environment

1) You already installed Ubuntu 16.04 

2) You have a nvidia GPU card (1080, 1080 Ti, etc) and installed nvidia driver as following: 
```bash
$sudo apt-get purge nvidia*
$sudo add-apt-repository ppa:graphics-drivers
$sudo apt-get install nvidia-384
$sudo reboot
```
3) Install Ubuntu system dependencies
```bash
$ sudo apt-get update
$ sudo apt-get upgrade

$ sudo apt-get install build-essential cmake git unzip pkg-config
$ sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev
$ sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
$ sudo apt-get install libxvidcore-dev libx264-dev
$ sudo apt-get install libgtk-3-dev
$ sudo apt-get install libhdf5-serial-dev graphviz
$ sudo apt-get install libopenblas-dev libatlas-base-dev gfortran
$ sudo apt-get install python-tk python3-tk python-imaging-tk

$ sudo apt-get install python2.7-dev python3-dev

$ sudo apt-get install linux-image-generic linux-image-extra-virtual
$ sudo apt-get install linux-source linux-headers-generic

```

4) Disable the Nouveau kernel driver

use nano to edit some file: 
```bash
$ sudo nano /etc/modprobe.d/blacklist-nouveau.conf
```
Add the following lines and then save and exit (ctrl+x):
```bash
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
```


Next let’s update the initial RAM filesystem and reboot the machine:
```bash
$ echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf
$ sudo update-initramfs -u
$ sudo reboot
```


5) Install CUDA 

You will want to download the CUDA Toolkit v8.0 via the [NVIDIA CUDA Toolkit website:](https://developer.nvidia.com/cuda-80-ga2-download-archive)
Once you’re on the download page, select Linux => x86_64 => Ubuntu => 16.04 => runfile (local) .
