# cuda_installation_guide

> This installation guide was put together after various failed installation attempts. I have used this same guide on multiple `Linux` machines e.g., servers, laptops, and desktops.

* configure and select the nvidia driver for cuda. check your version here: [cuda-compatibility-index](https://docs.nvidia.com/deploy/cuda-compatibility/index.html).

- cuda-toolkit-version    linux-x86_64-driver-version
  
  - CUDA 10.1 (10.1.105)	>= 418.39
  - CUDA 10.0 (10.0.130)	>= 410.48
  - CUDA 9.2 (9.2.88)		  >= 396.26
  - CUDA 9.1 (9.1.85)		  >= 390.46
  - CUDA 9.0 (9.0.76)		  >= 384.81

### CUDA DOWNLOADS

- [cuda-toolkit-archive](https://developer.nvidia.com/cuda-toolkit-archive)

- [cudnn-archive](https://developer.nvidia.com/rdp/cudnn-archive)


### GRAPHICS-DRIVER INSTALLATION:

* install the nvidia drivers from the following package:

```bash
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
sudo apt upgrade
```

* check the available drivers for your machine:

```bash
ubuntu-drivers list
```

* install the driver-version based on your cuda-toolkit-version:

```bash
sudo apt install nvidia-driver-410
```

* reboot your system now, before executing the commands below:

> **NOTE:** make sure the `gcc/g++` versions match the nvidia driver requirements.

```bash
sudo apt install gcc-7 g++-7
sudo apt update && sudo apt upgrade
```

### CUDA INSTALLATION:

* install dependancies:

```bash
sudo apt install linux-headers-$(uname  -r)
sudo apt update && sudo apt build-essential
sudo apt update && sudo apt install freeglut3 freeglut3-dev libxi-dev libxmu-dev
```

* installing cuda via run-file

> **NOTE:** don't install the nvidia driver from the cuda package. make sure you have the compatible graphics driver installed before installing cuda. In this case we have `nvidia-driver-410` installed and running for `cuda-10`.

```bash
sudo sh ./cuda_10.0.130_410.48_linux.run
```

> **NOTE:** if the installation **failed** due to the `gcc version`: add the parameter `--override`, and run `sudo sh ./cuda_10.0.130_410.48_linux.run --override` instead. this happens if you had a **previous cuda gcc version** installed. (we will properly take care of this after installing cuda, cudnn and configuring bashrc).
  
* configure enviroment path variables:

```bash
nano ~/.bashrc
```

* add the following to your bashrc file:

```bash
export PATH=/usr/local/cuda-10.0/bin:${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64:${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/extras/CUPTI/lib64
```

* apply changes:

```bash
source ~/.bashrc
```

### CUDNN INSTALLATION

* unzip cudnn package

```bash
tar -xzvf cudnn-10.0-linux-x64-v7.5.0.56.tgz cuda/
```

* copy cudnn files to the `include/` and `lib64/` directories:

```bash
sudo cp cuda/include/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

* link compatible `gcc` and `g++` version with cuda 10:

> **NOTE**: check which gcc and g++ versions you have installed first:

```bash
cd /usr/bin/
ls gcc*
ls g++*
```

* install compatible driver and gcc/g++ versions for cuda-10

```
sudo apt install gcc-7 g++-7
sudo apt update && sudo apt upgrade
```

* link compatible gcc & g++ --versions with new cuda installation

```bash
sudo ln -s /usr/bin/gcc-7 /usr/local/cuda/bin/gcc
sudo ln -s /usr/bin/g++-7 /usr/local/cuda/bin/g++
gcc --version
```

## CHECKING CUDA INSTALLATION

```
nvcc --version
```

* install cuda 10 samples:

```bash
cd NVIDIA_CUDA-10_Samples/
make
```

* run the demos:

```
cd bin/x86_64/linux/release/
./deviceQuery
./smokeParticles 
```

if you could run the demos with no issues then cuda is installed in your system.

## UNINSTALLING CUDA-TOOLKIT:

> you just need to run uninstall_cuda_< CUDA-VERSION >.pl to remove cuda completely from your system.

* navigate to the /bin directory

```bash
cd /usr/local/cuda-10/bin
```

* look for an uninstall cuda file (the name can be different depending on your cuda version)

```bash
ls
```

* run sudo and uninstall

```bash
sudo ./uninstall_cuda_10.pl
```

> **NOTE:** the `cuda-10/` folder might remain in `local/` so you can do the folling to fully delete everything (optional).

```bash
cd /usr/local/
ls
rm -r cuda-10
rm -r cuda
```
