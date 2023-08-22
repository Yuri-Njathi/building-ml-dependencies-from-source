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


# On Fedora (worked but the files have a torch_c issue)
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



## References
1. [PyTorch 1.7.0 and torchvision 0.8.0 builds for RaspberryPi 4 (32bit OS)](https://github.com/Kashu7100/pytorch-armv7l)

2. [Compling ARM stuff without an ARM board / Build PyTorch for the Raspberry Pi](https://nmilosev.svbtle.com/compling-arm-stuff-without-an-arm-board-build-pytorch-for-the-raspberry-pi)

3. [pytorch-arm-builds](https://github.com/nmilosev/pytorch-arm-builds)

4. [Torchvision](https://github.com/pytorch/vision)

5. [Tutorial: Running YOLOv5 Machine Learning Detection on a Raspberry Pi 4](https://jordan-johnston271.medium.com/tutorial-running-yolov5-machine-learning-detection-on-a-raspberry-pi-4-3938add0f719) 

6. [pytorch-rpi](https://github.com/ljk53/pytorch-rpi)

7. [Youtube Video : Install PyTorch and TorchVision on Raspberry pi computer ](https://www.youtube.com/watch?v=weHvI6j4OT8&t=142s)

8. [Install OpenCV on your Raspberry Pi](https://raspberrypi-guide.github.io/programming/install-opencv) 
