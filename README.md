# Usage of NVIDIA Containers
Check whether docker daemon is running on your platform.
If not please start with 
```  
sudo dockerd&
```  
and run
```  
docker run hello-world
```  
``` 
(base) [thomas@localhost ~]$ docker run -t --runtime=nvidia --rm nvidia/cuda:9.0-base-centos7 nvidia-smi
Unable to find image 'nvidia/cuda:9.0-base-centos7' locally
9.0-base-centos7: Pulling from nvidia/cuda
a02a4930cb5d: Pull complete 
ea81d4671bf2: Pull complete 
93458a2f72ef: Pull complete 
1d68675f2d7a: Pull complete 
4134a3a6a7c8: Pull complete 
Digest: sha256:2ef556769c4eaf28011c2105bfdaf1b72408b58c7ccfb76fa00c16a2dd15c753
Status: Downloaded newer image for nvidia/cuda:9.0-base-centos7
Wed Mar  6 12:16:15 2019       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 390.87                 Driver Version: 390.87                    |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 105...  Off  | 00000000:01:00.0  On |                  N/A |
| 30%   27C    P8    N/A /  75W |    273MiB /  4036MiB |      1%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
+-----------------------------------------------------------------------------+
``` 

``` 
More complete example 

docker run -t --runtime=nvidia --net=host --rm nvcr.io/nvidia/k8s/cuda-sample:nbody  nbody

if this error comes up
nvidia-docker: line 34: /usr/bin/docker: Permission denied
then
1093  sudo semanage fcontext -a -t container_runtime_exec_t /usr/bin/nvidia-docker
 1094  sudo restorecon -v /usr/bin/nvidia-docker
``` 
 
commands are shorter when using nvidia-docker

nvidia-docker run -t --net=host --rm nvcr.io/nvidia/k8s/cuda-sample:nbody  nbody


Another example
Caffe2 is a deep-learning framework designed to easily express all model types, for example, CNN, RNN, and more, in a friendly python-based API, and execute them using a highly efficiently C++ and CUDA back-end.

docker pull nvcr.io/nvidia/caffe2:18.08-py3

Running Caffe2

nvidia-docker run -it --rm -v /home/thomas/data/mnist:/data/mnist nvcr.io/nvidia/caffe2:18.08-py3

git clone --recursive https://github.com/caffe2/tutorials caffe2_tutorials

cd nvidia-examples/cifar10
``` 
./get_cifar10.sh
./make_cifar10.sh
python train_cifar10.py
``` 
...
...
Test loss: 0.814758, accuracy: 0.724100
Epoch 10/10, iteration  100/500, loss=0.706333
Epoch 10/10, iteration  200/500, loss=0.846863
Epoch 10/10, iteration  300/500, loss=0.536812
Epoch 10/10, iteration  400/500, loss=0.571494
Epoch 10/10, iteration  500/500, loss=0.742528
Test loss: 0.802806, accuracy: 0.729100
``` 


All Containers can be found here:
# NVIDIA GPU CLOUD
login to https://ngc.nvidia.com/containers and check for NVIDIA containers

# Create API Key
read https://ngc.nvidia.com/configuration/api-key
<img src="https://github.com/schoenemeyer/docker-nvidia-basics/blob/master/gpucloud.png" width="502">

# Login to NGC
docker login nvcr.io
Username: $oauthtoken
Password: <your API key>
```  
[thomas@localhost ~]$ docker images
REPOSITORY                  TAG                 IMAGE ID            CREATED             SIZE
thomassch/myfirstapp1       latest              3e7ecfb50f90        7 weeks ago         57MB
thomassch/myfirstapp        latest              3e7ecfb50f90        7 weeks ago         57MB
nginx                       latest              568c4670fa80        2 months ago        109MB
ubuntu                      latest              93fd78260bd1        2 months ago        86.2MB
nvidia/cuda                 latest              1cc6f1613121        2 months ago        2.24GB
alpine                      3.5                 dc496f71dbb5        4 months ago        4MB
alpine                      latest              196d12cf6ab1        4 months ago        4.41MB
hello-world                 latest              4ab4c602aa5e        4 months ago        1.84kB
nvcr.io/nvidia/tensorflow   18.07-py3           8289b0a3b285        7 months ago        3.34GB
nvcr.io/nvidia/pytorch      17.12               5ac6ff8f9a81        14 months ago       4.52GB
dockersamples/static-site   latest              f589ccde7957        2 years ago         191MB
```  
