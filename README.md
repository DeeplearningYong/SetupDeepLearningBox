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

Move the downloaded runfile into your home folder, then unpack it:
```bash
$ cd ~ 
$ chmod +x cuda_8.0.61_375.26_linux.run
$ mkdir installers
$ sudo ./cuda_8.0.61_375.26_linux.run -extract=`pwd`/installers

$ modprobe nvidia

$ cd installers
$ sudo ./cuda-linux64-rel-8.0.61-21551265.run
$ sudo ./cuda-samples-linux-8.0.61-21551265.run
```

Again, accepting the licenses and following the default prompts. 
You may have to press ‘space’ to scroll through the license agreement and then enter “accept”. 
When it asks you for installation paths, just press <enter>  to accept the defaults.


Now that the NVIDIA CUDA driver and tools are installed, you need to update your ~/.bashrc  file to include CUDA Toolkit
```bash
nano ~/.bashrc
```
and add following lines in the end
```bash
# NVIDIA CUDA Toolkit
export PATH=/usr/local/cuda-8.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64/
```

Now, reload your ~/.bashrc 
```bash
$source ~/.bashrc
```
and then test the CUDA Toolkit installation by compiling the deviceQuery  example program and running it:
```bash
$ cd /usr/local/cuda-8.0/samples/1_Utilities/deviceQuery
$ sudo make
$ ./deviceQuery
```
if you installed everything correctly, you will observe 
```bash
Result = PASS
```

6) Install cuDNN

For this step, you will need to [Create a free account](https://developer.nvidia.com/developer-program) with NVIDIA and [download cuDNN](https://developer.nvidia.com/cudnn).

You can download => Download cuDNN v5.1 (Jan 20, 2017), for CUDA 8.0 => cuDNN v5.1 Library for Linux



