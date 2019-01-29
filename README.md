# docker-nvidia-basics


sudo dockerd

check docker daemon running?

docker run hello-world
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
