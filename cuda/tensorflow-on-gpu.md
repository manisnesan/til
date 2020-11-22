## Run tensorflow on GPU using nvidia CUDA

### Environment
OS: Fedora 29
CUDA: 10.1
tensorflow: 2.3.1


### Diagnosis

- verify the installation using `$ nvidia-smi`

```
$ nvidia-smi
Mon Nov 16 11:59:35 2020       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 440.95.01    Driver Version: 440.95.01    CUDA Version: 10.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 1080    Off  | 00000000:01:00.0 Off |                  N/A |
|  0%   54C    P0    40W / 180W |      0MiB /  8116MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```

- Verify cuda version
```
$ nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2019 NVIDIA Corporation
Built on Wed_Oct_23_19:24:38_PDT_2019
Cuda compilation tools, release 10.2, V10.2.89
```

- Check if tensorflow can detect the GPU
```
>>> import tensorflow as tf;print(tf.__version__)
2.2.0

>>> tf.test.gpu_device_name()
```
Empty output indicates that tensorflow is not detecting GPU

### RootCause

- Installing incorrect tensorflow package
- Incorrect/Unsupported cuda toolkit versions for the target platform
- Missing dynamic libraries

### Resolution
- Ensure you have cuda-enabled GPU

``` $ lspci | grep -i nvidia
01:00.0 VGA compatible controller: NVIDIA Corporation GP104 [GeForce GTX 1080] (rev a1)
01:00.1 Audio device: NVIDIA Corporation GP104 High Definition Audio Controller (rev a1)
```

- Download and install __cuda toolkit__ and cuDNN based on supported versions and according to your target platform

![Img](https://github.com/easy-tensorflow/easy-tensorflow/blob/master/0_Setup_TensorFlow/files/cuda_linux.png)

Note: Ensure tensorflow supports the cuda version you are using before installing cuda toolkit. Eg: tensorflow did not work in cuda 10.2 but only 10.1 on Fedora 29 as of Nov 23, 2020.

```
$ sudo dnf config-manager --add-repo http://developer.download.nvidia.com/compute/cuda/repos/fedora29/x86_64/cuda-fedora29.repo
$ sudo dnf clean all
$ sudo dnf -y install cuda
```

- You can check the cuda installation path $CUDA_PATH using
```
$ which nvcc
$ ldconfig -p | grep cuda
```
$CUDA_PATH could be in /usr... or /usr/local/cuda/ or /usr/local/cuda/cuda-10.1/ . Add the path to your .bashrc or .zshrc file

```
export CUDA_ROOT=/usr/local/cuda/bin
export LD_LIBRARY_PATH=/usr/local/cuda/lib64/
export PATH=$PATH:$CUDA_ROOT
```
Run `source ~/.zshrc`

- Download and install cuDNN  from  https://developer.nvidia.com/cudnn after signing up for nvidia developer program. In my case, for 10.1 I was missing 'libcudnn.so.7, so I had to download the rpm libcudnn7-7.6.5.32-1.cuda10.1.x86_64.rpm

- Installing gpu specific tensorflow package

- `$ pip install tensorflow-gpu` # This installs tensorflow 2.3.1 version

- Verify the installation

```
>>> import tensorflow as tf;print(tf.__version__);tf.test.gpu_device_name()
2.3.1
'/device:GPU:0'
```

## Resources
- [Install cuda and cuDNN](https://www.easy-tensorflow.com/tf-tutorials/install/cuda-cudnn)
- [Diagnosis Gist](https://gist.github.com/manisnesan/d79681ebffca4579a09c56381c0e642a)
