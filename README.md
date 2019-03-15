# Usage of NVIDIA Containers pulled from NVIDIA GPU Cloud (NGC)
https://ngc.nvidia.com/catalog/landing
For access you have to register (see below)   

This test was done with the following HW and SW    

Docker Version 18.09.0   
NVIDIA Driver 390.87   
CentOS Linux release 7.6.1810 (Core)   
1x AMD FX(tm)-6300 Six-Core Processor   
GeForce GTX 1050Ti  

First, check if your docker daemon is running with 
``` 
ps -ef | grep docker    
```
If the output is empty please start with 
```  
sudo dockerd&
```  
and do a test run
```  
docker run hello-world
```  
Now lets start with pulling a Nvidia Container from Nvidia NGC and start nvidia-smi in your container

```  
docker pull  nvcr.io/nvidia/cuda:9.0-base-centos7
```  
or start directly with 
```  
docker run -t --runtime=nvidia --rm nvidia/cuda:9.0-base-centos7 nvidia-smi
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
Lets move on with some more comprehensive example, we will run the famous nbody example from the Cuda Samples in a container

``` 
docker run -t --runtime=nvidia --net=host --rm nvcr.io/nvidia/k8s/cuda-sample:nbody  nbody
``` 
(Side remark) if this error comes up
``` 
nvidia-docker: line 34: /usr/bin/docker: Permission denied
``` 
then you should run the two lines below:
``` 
sudo semanage fcontext -a -t container_runtime_exec_t /usr/bin/nvidia-docker
sudo restorecon -v /usr/bin/nvidia-docker
``` 
 
instead of docker you can run commands using nvidia-docker directly and it is a bit shorter. Nvidia-docker is ust a wrapper that sets up the nvidia runtime to be the default when invoking docker commands.
``` 
nvidia-docker run -t --net=host --rm nvcr.io/nvidia/k8s/cuda-sample:nbody  nbody
``` 
This will open a new windows with the animated results of nbody.

Lets try another example for Deep Learning. Caffe2 is a deep-learning framework designed to easily express all model types, for example, CNN, RNN, and more, in a friendly python-based API, and execute them using a highly efficiently C++ and CUDA back-end. We get the container
``` 
docker pull nvcr.io/nvidia/caffe2:18.08-py3
``` 
Then we start running the Caffe2 container by 
``` 
nvidia-docker run -it --rm -v /home/thomas/data/mnist:/data/mnist nvcr.io/nvidia/caffe2:18.08-py3
``` 
Or starting the Tensorflowcontainer with 
``` 
nvidia-docker run -it --rm -v /home/thomas/data/tf:/data/tf nvcr.io/nvidia/tensorflow:19.02-py3
``` 
We can get the tutorial samples also 
``` 
git clone --recursive https://github.com/caffe2/tutorials caffe2_tutorials
cd nvidia-examples/cifar10

./get_cifar10.sh
./make_cifar10.sh
python train_cifar10.py


Test loss: 0.814758, accuracy: 0.724100
Epoch 10/10, iteration  100/500, loss=0.706333
Epoch 10/10, iteration  200/500, loss=0.846863
Epoch 10/10, iteration  300/500, loss=0.536812
Epoch 10/10, iteration  400/500, loss=0.571494
Epoch 10/10, iteration  500/500, loss=0.742528
Test loss: 0.802806, accuracy: 0.729100
``` 

All Containers can be found here:
## NVIDIA GPU CLOUD
login to https://ngc.nvidia.com/containers and check for NVIDIA containers

## Create API Key
read https://ngc.nvidia.com/configuration/api-key
<img src="https://github.com/schoenemeyer/docker-nvidia-basics/blob/master/gpucloud.png" width="502">

## Login to NGC
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

## Container compatible to Driver 390.87

nvcr.io/nvidia/cuda:9.0-devel-ubuntu16.04    
nvcr.io/nvidia/inferenceserver:18.08.1-py3     
nvcr.io/nvidia/torch:18.08-py2    
nvcr.io/nvidia/theano:18.08     
nvcr.io/hpc/gromacs:2018.2     
nvcr.io/hpc/lattice-microbes:2018.03   
nvcr.io/hpc/lattice-microbes:2018.03    


## Running Jupyter on a remote docker container via SSH

If you want to use Jupyter locally on your notebook while Jupyter web-app is running the code in a remote machine, you should read this post. The background is explained in detailed    
https://medium.com/@lucasrg/using-jupyter-notebook-running-on-a-remote-docker-container-via-ssh-ea2c3ebb9055    
The image provided by the author explains it.
<img src="https://github.com/schoenemeyer/docker-nvidia-basics/blob/master/1%20TAC174dy1N_boaMkCOKrtg.png" width="352">
## Quick Guide

ssh in the remote machine with –L 
``` 
thomass@THOMASS-LT1:~$ ssh -L 9999:localhost:9999 thomas@<yourgpuserver>.nvidia.com
``` 
Start the nvidia-docker on the GPU machine (incl. mounting your current working directory.
``` 
nvidia-docker run -it -p 9999:9999  --rm -v `pwd`:`pwd` -w `pwd` nvcr.io/nvidia/tensorflow:18.08-py3
``` 
Start jupyter
(an optional install might be required) pip install jupyter
``` 
root@6b2596d4f4cb:/workspace# jupyter notebook --ip 0.0.0.0 --port 9999 --allow-root
``` 
Open browser on your local machine and open localhost:9999
login with your token
Done.
<img src="https://github.com/schoenemeyer/docker-nvidia-basics/blob/master/Capture.PNG" width="552">
