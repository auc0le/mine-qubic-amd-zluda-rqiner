# mine-qubic-amd-zluda-rqiner
Instructions on how to mine Qubic crypto on AMD GPUs with rqiner!

I got this working on 4/5/2024 and I am getting 49.5 it/s on a Radeon 780M.

I assume you are running Ubuntu, have a ZLUDA compatible AMD GPU, and are working in your homedir.

# 1. Install ZLUDA
## 1.1 Prerequisites
### 1.1.1 Git
### 1.1.2 CMake
### 1.1.3 Python3
### 1.1.4 Rust
### 1.1.5 ROCm 5.7 or higher
  Using Ubuntu 22.04:
  ```
    sudo apt install "linux-headers-$(uname -r)" "linux-modules-extra-$(uname -r)"
    # See prerequisites. Adding current user to Video and Render groups
    sudo usermod -a -G render,video $LOGNAME
    wget https://repo.radeon.com/amdgpu-install/6.0.2/ubuntu/jammy/amdgpu-install_6.0.60002-1_all.deb
    sudo apt install ./amdgpu-install_6.0.60002-1_all.deb
    sudo apt update
    sudo apt install amdgpu-dkms
    sudo apt install rocm
    echo "Please reboot system for all settings to take effect."
  ```

## 1.2 Build ZLUDA
  Link to great ZLUDA project that makes this possible: https://github.com/vosen/ZLUDA
## 1.2.1 Check out the git
  ```git clone --recurse-submodules https://github.com/vosen/zluda.git```
## 1.2.2 Compile
  Change into the zluda dir and compile
  ```
  cd zluda/
  cargo xtask --release
  ```
If your build fails, read the error message and resolve any issues.

## 1.2.3 Move build
Once your build has succeeded, you will want to move your target\release folder to a permanent location:
```
mv ./target/release /usr/lib/zluda
 ```

## 1.3 Download the CUDA version of rqiner
 Link to great rqiner project: [https://github.com/Qubic-Solutions/rqiner-builds/releases/tag/v0.3.19-cuda-beta](https://github.com/Qubic-Solutions/rqiner-builds)
from your home dir (or wherever you want to run it):
```
wget https://github.com/Qubic-Solutions/rqiner-builds/releases/download/v0.3.19-cuda-beta/rqiner-x86-cuda
 ```
