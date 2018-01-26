
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

$ sudo modprobe nvidia

$ cd installers
$ sudo ./cuda-linux64-rel-8.0.61-21551265.run
$ sudo ./cuda-samples-linux-8.0.61-21551265.run
```

Again, accepting the licenses and following the default prompts. 
You may have to press ‘space’ to scroll through the license agreement and then enter “accept”. 
When it asks you for installation paths, just press <enter>  to accept the defaults. For example: 
```bash
accept/decline/quit: accept             
Logging to /tmp/cuda-installer-2532

Enter install path
 [ default is /usr/local/cuda-8.0 ]: 

Would you like to add desktop menu shortcuts?
(y)es/(n)o/(a)ll users [ default is all users ]: n

Would you like to create a symbolic link /usr/local/cuda pointing to /usr/local/cuda-8.0?
(y)es/(n)o/(a)bort [ default is yes ]: y

Creating symbolic link /usr/local/cuda -> /usr/local/cuda-8.0
```


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
$ sudo make -j8
$ ./deviceQuery
```
if you installed everything correctly, you will observe 
```bash
Device 0: "GeForce GTX 1080 Ti"
  CUDA Driver Version / Runtime Version          9.0 / 8.0
......
......
......
Result = PASS
```

6) Install cuDNN

For this step, you will need to [Create a free account](https://developer.nvidia.com/developer-program) with NVIDIA and [download cuDNN](https://developer.nvidia.com/cudnn).

You can download => Download cuDNN v5.1 (Jan 20, 2017), for CUDA 8.0 => cuDNN v5.1 Library for Linux

Again, you can move the downloaded file into your home folder, then
```bash
$ cd ~
$ tar -zxf cudnn-8.0-linux-x64-v5.1.tgz
$ cd cuda
$ sudo cp -P lib64/* /usr/local/cuda/lib64/
$ sudo cp -P include/* /usr/local/cuda/include/
$ cd ~
```

7) Install python pip, numpy
```bash
$ wget https://bootstrap.pypa.io/get-pip.py
$ sudo python get-pip.py
$ sudo pip install numpy
```

8) Install OpenCV
```bash
$ cd ~
$ wget -O opencv.zip https://github.com/Itseez/opencv/archive/3.3.0.zip
$ wget -O opencv_contrib.zip https://github.com/Itseez/opencv_contrib/archive/3.3.0.zip

$ unzip opencv.zip
$ unzip opencv_contrib.zip

$ cd ~/opencv-3.3.0/
$ mkdir build
$ cd build

$ cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D WITH_CUDA=OFF \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib-3.3.0/modules \
    -D BUILD_EXAMPLES=ON ..

$ sudo make -j8

$ sudo make install
$ sudo ldconfig
$ cd ~

```

Test whether opencv is installed correctly or not:
```bash
$ python
>>> import cv2
>>> cv2.__version__
'3.3.0'
```

# Build Caffe with SSD 

Install system dependencies
```bash
sudo apt-get install -y build-essential cmake git pkg-config
sudo apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install -y libatlas-base-dev 
sudo apt-get install -y --no-install-recommends libboost-all-dev
sudo apt-get install -y libgflags-dev libgoogle-glog-dev liblmdb-dev

sudo apt-get install -y python-pip
sudo apt-get install -y python-dev
sudo apt-get install -y python-numpy python-scipy
```

```bash
sudo apt-get install libtcmalloc-minimal4
###add following line into ~/.bashrc
export LD_PRELOAD="/usr/lib/libtcmalloc_minimal.so.4"
```
then 
```bash
source ~/.bashrc
```

Get the code:
```bash
Ask me to get the code for Caffe ... 
unzip it

cd caffe
cd python
for req in $(cat requirements.txt); do sudo pip install $req; done

 
cd ..
sudo make clean
sudo make all -j8
sudo make test -j8 
sudo make pycaffe -j8
```




