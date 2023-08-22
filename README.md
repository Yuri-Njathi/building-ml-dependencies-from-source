# building-ml-dependencies-from-source

# On the Raspberry Pi 32-bit
## 1. PyTorch and Torchvision
### Dependencies
`sudo apt install libopenblas-dev libblas-dev m4 cmake cython python3-dev python3-yaml python3-setuptools`

### Build Options
`export NO_CUDA=1
export NO_DISTRIBUTED=1
export NO_MKLDNN=1 
export BUILD_TEST=0
export MAX_JOBS=4`

### Compile
The wheel file will be created in pytorch/dist/* after running the following commands.

#### PyTorch
A.
`git clone https://github.com/pytorch/pytorch --recursive && cd pytorch
git checkout v1.7.0
git submodule update --init --recursive
python setup.py bdist_wheel`

B.
Install the latest Raspberry Pi OS (32-bit).
Run `sudo apt update && sudo apt upgrade`.
Run `git clone git@github.com:ljk53/pytorch-rpi && cd pytorch-rpi`.
Run `LIBTORCH_VARIANT=armv7l-cxx11-abi-shared-without-deps ./build_libtorch.sh`.

#### Torchvision
`git clone https://github.com/pytorch/vision && cd vision
git checkout v0.8.1
git submodule update --init --recursive
python setup.py bdist_wheel`


## 2. OpenCV-Python
[7]

`sudo apt-get update`

`sudo apt-get install build-essential cmake pkg-config libjpeg-dev libtiff5-dev libjasper-dev libpng-dev libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libxvidcore-dev libx264-dev libfontconfig1-dev libcairo2-dev libgdk-pixbuf2.0-dev libpango1.0-dev libgtk2.0-dev libgtk-3-dev libatlas-base-dev gfortran libhdf5-dev libhdf5-serial-dev libhdf5-103 python3-pyqt5 python3-dev -y`

`pip install opencv-python==4.7.0.72`


# On Fedora (worked but the files have a torch_c issue) [2]

## Preparation

1. Install packages: qemu,  qemu-user and Virt manager

`sudo dnf install qemu-system-arm qemu-user-static virt-manager`

2. Install rootfs to emulate raspberry pi architecture

`sudo dnf install --releasever=30 --installroot=/tmp/F30ARM --forcearch=armv7hl --repo=fedora --repo=updates systemd passwd dnf fedora-release vim-minimal openblas-devel blas-devel m4 cmake python3-Cython python3-devel python3-yaml python3-pillow python3-setuptools python3-numpy python3-cffi python3-wheel gcc-c++ tar gcc git make tmux -y`

This will install a ARM rootfs to your /tmp directory along with everything you need to build PyTorch.
You can access it by opening file explorer in Fedora -> Other Locations -> Fedora Linux -> tmp -> F30ARM, here all files are located within.

3. Use chroot to access it.

`sudo chroot /tmp/F30ARM`

 4. You are now within the "ARM board". Verify your kernel arch:
In the documentation they got

`Linux toshiba-x70-a 5.1.12-300.fc30.x86_64 #1 SMP Wed Jun 19 15:19:49 UTC 2019 armv7l armv7l armv7l GNU/Linux`

I got

`Linux fedora 6.2.9-300.fc38.x86_64 #1 SMP PREEMPT_DYNAMIC Thu Mar 30 22:32:58 UTC 2023 armv7l armv7l armv7l GNU/Linux`

Then run a few commands to make the system work:

`/usr/lib/python3.7/site-packages/dnf/rpm/__init__.py` 

I got an error for this.

`usr/lib/python3.7/site-packages/dnf/rpm/__init__.py: Permission denied`

These worked though:

`alias dnf='dnf --releasever=30 --forcearch=armv7hl --repo=fedora --repo=updates'`

`alias python=python3`

`echo 'nameserver 8.8.8.8' > /etc/resolv.conf`

`python V -> Python 3.7.7`

## 1. PyTorch and Torchvision

#### A. PyTorch

Clone pytorch repository:

`git clone https://github.com/pytorch/pytorch --recursive && cd pytorch`

Select pytorch version you want to build from source here I select version 1.9.0
`git checkout v1.9.0`

Synchronize pytorch version with it's dependencies and update the dependencies

`git submodule sync`

`git submodule update --init --recursive`

Set building parameters

`export NO_CUDA=1`

`export NO_DISTRIBUTED=1`

`export NO_MKLDNN=1`

`export BUILD_TEST=0 # for faster builds`

`export MAX_JOBS=8` # I had 8 cores in one machine and 4 in another # To find the number of cores I used `lscpu`

`# export NO_NNPACK=1 # update July 19, this is optional, can build with NNPACK`

`# export NO_QNNPACK=1 # same as above can be omitted`

Build the Pytorch wheel; took about 2 and a half hours for the 8-core machine
`python setup.py bdist_wheel`

the wheel will be inside `dist/` folder within the `pytorch/` folder.

Mount disk # Why??
`mount -o bind /dev /tmp/F30ARM/dev`

B. #### Torchvision

Install pytorch
`pip3 install ../pytorch/dist/torch-1.9.0a0+gitd69c22d-cp37-cp37m-linux_armv7l.whl`

## References
1. [PyTorch 1.7.0 and torchvision 0.8.0 builds for RaspberryPi 4 (32bit OS)](https://github.com/Kashu7100/pytorch-armv7l)

2. [Compling ARM stuff without an ARM board / Build PyTorch for the Raspberry Pi](https://nmilosev.svbtle.com/compling-arm-stuff-without-an-arm-board-build-pytorch-for-the-raspberry-pi)

3. [pytorch-arm-builds](https://github.com/nmilosev/pytorch-arm-builds)

4. [Torchvision](https://github.com/pytorch/vision)

5. [Tutorial: Running YOLOv5 Machine Learning Detection on a Raspberry Pi 4](https://jordan-johnston271.medium.com/tutorial-running-yolov5-machine-learning-detection-on-a-raspberry-pi-4-3938add0f719) 

6. [pytorch-rpi](https://github.com/ljk53/pytorch-rpi)

7. [Youtube Video : Install PyTorch and TorchVision on Raspberry pi computer ](https://www.youtube.com/watch?v=weHvI6j4OT8&t=142s)

8. [Install OpenCV on your Raspberry Pi](https://raspberrypi-guide.github.io/programming/install-opencv) 
