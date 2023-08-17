# building-ml-dependencies-from-source

# Raspberry Pi 32-bit
1. PyTorch and Torchvision
## Dependencies
`sudo apt install libopenblas-dev libblas-dev m4 cmake cython python3-dev python3-yaml python3-setuptools`

## Build Options
`export NO_CUDA=1
export NO_DISTRIBUTED=1
export NO_MKLDNN=1 
export BUILD_TEST=0
export MAX_JOBS=4`

## Compile
The wheel file will be created in pytorch/dist/* after running the following commands.

### PyTorch
`git clone https://github.com/pytorch/pytorch --recursive && cd pytorch
git checkout v1.7.0
git submodule update --init --recursive
python setup.py bdist_wheel`

### Torchvision
`git clone https://github.com/pytorch/vision && cd vision
git checkout v0.8.1
git submodule update --init --recursive
python setup.py bdist_wheel`



# References
1. [PyTorch 1.7.0 and torchvision 0.8.0 builds for RaspberryPi 4 (32bit OS)](https://github.com/Kashu7100/pytorch-armv7l)

2. [Compling ARM stuff without an ARM board / Build PyTorch for the Raspberry Pi](https://nmilosev.svbtle.com/compling-arm-stuff-without-an-arm-board-build-pytorch-for-the-raspberry-pi)

3. [pytorch-arm-builds](https://github.com/nmilosev/pytorch-arm-builds)
