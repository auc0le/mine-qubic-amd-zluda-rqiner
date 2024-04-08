# mine-qubic-amd-zluda-rqiner
Instructions on how to mine Qubic crypto on AMD GPUs with rqiner!

I got this working on 4/5/2024 and I am getting 49.5 it/s on a Radeon 780M.

Sample results:
| GPU | Performance |
| ------------- | ------------- |
| Radeon 780M  | 50 it/s  |
| RX 6600  | 95 it/s  |
| RX 5700 XT  | 105 it/s  |
| RX 6800 XT  | 309 it/s  |
| RX 7900 XTX | 467 it/s |


I assume you:
  - are running Ubuntu 22.04,
    - Hive OS might work if you remove existing version first:
      ``apt purge hive-amdgpu-x.x.x/rocm-x.x.x``
    - You want to go from driver ver 5.4.6--->6.3.2401.0 under 6.1.0 Hiveos(beta ver) based on Ubuntu 20     
  - have a ZLUDA [compatible AMD GPU](https://rocm.docs.amd.com/projects/install-on-linux/en/latest/reference/system-requirements.html), 
  - are working in your homedir, 
  - and you are already root (sudo su).

NOTE: WSL on windows does NOT support pure GPU passthru so ZLUDA will NOT work on Windows WSL.  Some user's have reported getting this ZLUDA setup to work in base windows and that is beyond the scope of this document but here is what they said: need to have zluda.exe and the dlls it came with in the same directory as the rqiner and load it through powershell.

# 1. Install ZLUDA
## 1.1 Prerequisites
best to update your apt before starting:
```
apt update
```
### 1.1.1 Git
```
apt-get install git-all
```
### 1.1.2 CMake
```
apt install cmake
```
### 1.1.3 Python3
```
apt install python3
```
### 1.1.4 Rust
```
apt install rustc
```
### 1.1.5 C++
```
apt install g++
```
### 1.1.6 ROCm 5.7 or higher
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
### 1.1.7 Reboot for the changes to take effect!

## 1.2 Build ZLUDA
  Link to great ZLUDA project that makes this possible: https://github.com/vosen/ZLUDA
  
  Note that to save time, you might try to use a release instead of building ZLUDA but building will make sure you have all the dependancies.
## 1.2.1 Check out the git
  ```git clone --recurse-submodules https://github.com/vosen/zluda.git```
## 1.2.2 Compile (this can take a while, esp if you are CPU mining in the background)
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

# 2. Run rqiner to mine!
  ATM this doesn't seem to work with the qli-client, so rqiner to the rescue!

## 2.1 Download the CUDA version of rqiner
 Link to great rqiner project: https://github.com/Qubic-Solutions/rqiner-builds
 
from your home dir (or wherever you want to run it):
```
wget https://github.com/Qubic-Solutions/rqiner-builds/releases/download/v0.3.22/rqiner-x86-cuda
 ```

## 2.2 Make the file executable 
```
  chmod +x rqiner-x86-cuda
```

# 3. Now let's mine some qubic!
```
 LD_LIBRARY_PATH="/usr/lib/zluda/:$LD_LIBRARY_PATH" ./rqiner-x86-cuda -i MENJJUJDUNWGYEOEMGAHXBTAOFOCQDQTHRYRZDGYPBNYECWASFPLUIJHMHLC -l GMK-K4-GPU
 ```

You will want to change the address or you will be mining for me!  And you may want to change the label or it will look like you are using the little mini PC that I have for mining but you should be up and running!

Example output:
```
[2024-04-05T18:14:56Z INFO  rqiner] Rust > C++ ;)
[2024-04-05T18:14:56Z INFO  rqiner] rqiner v0.3.19
[2024-04-05T18:14:56Z INFO  rqiner] Running Pool Miner
[2024-04-05T18:14:56Z INFO  rqiner] Solution Threshold: 43 | Random Seed: [150, 3, 18, 211, 186, 100, 124, 91, 52, 76, 230, 86, 115, 50, 143, 244, 59, 16, 138, 136, 32, 221, 207, 243, 235, 37, 170, 20, 181, 147, 217, 182]
[2024-04-05T18:14:56Z INFO  rqiner::gpu::pool] Mounted GPU0 AMD Radeon Graphics [ZLUDA] 3GB
[2024-04-05T18:15:04Z INFO  rqiner::gpu::pool] GPU0 AMD Radeon Graphics [ZLUDA]: 50.10 it/s
[2024-04-05T18:15:04Z INFO  rqiner::gpu::pool] TQCBV...FQVAN | Total Iterrate: 50.10 it/s | Average(10): 50.10 it/s | Solutions: 0
```
# 4. Optional tip
You can send some qubic to me as a thanks if you get it working :) 

My address is: MENJJUJDUNWGYEOEMGAHXBTAOFOCQDQTHRYRZDGYPBNYECWASFPLUIJHMHLC 
