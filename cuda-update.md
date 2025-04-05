# Upgrade CUDA Version(Driver and Toolkit)
mainly for host, the same for container except driver version.
* `ls -alh /usr/local/*` or `nvcc --version`to check CUDA version
* `cat /proc/driver/nvidia/version` to check driver version
* `nvidia-smi` to check tool version. if there's driver and toolkit version dismatch issue, it will still show its version.

```
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 550.144.03             Driver Version: 550.144.03     CUDA Version: 12.4     |
|-----------------------------------------+------------------------+----------------------+

$ nvidia-smi
Failed to initialize NVML: Driver/library version mismatch
NVML library version: 535.230

```
## Update CUDA
* Upgrade from 12.2 to 12.4 on Ubuntu 20.04.
* Search 'NCCL download' in google. and select the version. find the link: https://developer.nvidia.com/cuda-toolkit-archive
  * Use network installer, not use runfile, because it's quiet, and maybe encounter the driver in use issue.
    * Once encounter the driver in use issue, use `lsof /dev/nvidia*` to see who are using devices, `pstree` to find services, use `systemctl stop xxxx` to them, and `rmmod xx` to stop drivers.
    *   Note: `systemctl list-units --type=service` to list all services
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update
sudo apt-get -y install cuda-toolkit-12-4
# Update Driver version, it's related to cuda toolkit, and toolkit should match the cuda driver version
# cat /proc/driver/nvidia/version to see kernel driver version, or nvidia-smi to see the driver version
# nvcc etc. to see the cuda's version
# +-----------------------------------------------------------------------------------------+
# | NVIDIA-SMI 550.144.03             Driver Version: 550.144.03     CUDA Version: 12.4     |
#|-----------------------------------------+------------------------+----------------------+
sudo apt-get install -y cuda-drivers-550

# have to install nvidia-fabricmanager and start the service in hopper GPUs, otherwise, the GPU woundn't be initialized correctly.
# could check it in python's torch `import torch; torch.cuda.is_available()`
sudo apt install nvidia-fabricmanager-550
sudo apt install nvidia-fabricmanager-dev-550
sudo apt install cuda-drivers-fabricmanager-550
sudo apt install libnvidia-nscq-550

sudo systemctl start nvidia-fabricmanager
```
* remove/downgrade if there's problem
  * please compare 12.2 in the offical download page, or just `apt-cache search xx` to check the versions. 
```
sudo apt autoremove cuda-toolkit-12-4
sudo apt autoremove cuda-drivers-550

wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb

sudo apt-get update
sudo apt-get -y install cuda-toolkit-12-2
sudo apt-get install -y cuda-drivers-535

sudo apt install nvidia-fabricmanager-535
sudo apt install nvidia-fabricmanager-dev-535
sudo apt install cuda-drivers-fabricmanager-535
sudo apt install libnvidia-nscq-535

```

* After installation, please set
  * Normally, after above installation steps,  /user/loca/cuda will be symbolly linked to /user/loca/cuda-12.4

```
===========
= Summary =
===========

Driver:   Installed
Toolkit:  Installed in /usr/local/cuda-12.4/

Please make sure that
 -   PATH includes /usr/local/cuda-12.4/bin
 -   LD_LIBRARY_PATH includes /usr/local/cuda-12.4/lib64, or, add /usr/local/cuda-12.4/lib64 to /etc/ld.so.conf and run ldconfig as root

To uninstall the CUDA Toolkit, run cuda-uninstaller in /usr/local/cuda-12.4/bin
To uninstall the NVIDIA Driver, run nvidia-uninstall
Logfile is /var/log/cuda-installer.log
-----
```
* Resolve driver and toolkit dismatch issue
  * Normally it's caused by lower driver version, if the driver verison is greater, there will be no problem.
  * if the major driver version is 535.xx.xx
```
sudo apt install cuda-drivers-535 
```


## Update NCCL
* Search 'NCCL download' in google. and select the version. Follow the instructions to install.
  * Actually, `https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.1-1_all.deb` is the same with CUDA apt link above, don't need to do it again.
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update
sudo apt install libnccl2=2.25.1-1+cuda12.4 libnccl-dev=2.25.1-1+cuda12.4
```
* `find /user -name "*nccl*"` to find the libnccl's version to check nccl's version

## Update cuDNN
* Search 'cuDNN download' in google. and select the version
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update
sudo apt-get -y install cudnn
sudo apt-get -y install cudnn-cuda-12
```

## Torch issue
* please `pip list | grep nccl` to check `nvidia-nccl-cu12`'s version, it should be the same with the NCCL version above
  * Otherwise, there will be nccl issue while running torch
