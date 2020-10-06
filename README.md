# dockerfile-gpu-opencv-jupyter

## How to use
1. Create base image
    - Go to Dockerfile directory: `$ cd 01_core_dockerfile`
    - Build image: `$ docker build -t ardihikaru/nvidia-gpu-opencv:1.0 .`
2. Create jupyter image
    - Go to Dockerfile directory: `$ cd 02_jupyter_dockerfile`
    - Build image: `$ docker build -t ardihikaru/jupyter:1.0 .`
3. Deploy jupyter container
    - Run container*
        - **CPU Version**: `$ docker run --name my-jupyter -d -p 8888:8888 -v <desired_path>/src/:/src/ ardihikaru/jupyter:1.0`
        - **GPU Version** (You need to install [NVIDIA-Docker2](https://cnvrg.io/how-to-setup-docker-and-nvidia-docker-2-0-on-ubuntu-18-04/)): 
            `$ docker run --runtime=nvidia --name my-jupyter -d -p 8888:8888 -v <desired_path>/src/:/src/ ardihikaru/jupyter:1.0`
    
        ***For example here**, I changed `<desired_path>` into this path: `/home/ardi/my-jupyter`
    - Make sure that the container is running (it will say `Up xx minutes`):
        `$ docker container ps -a | grep my-jupyter`
    - In this case, you can freely add and modify your project in your host computer. :)
 4. Basically, you already able to access the jupyter in this URL: **[http://localhost:8888/](http://localhost:8888/)**
 5. To login, you need to get the `token` by loging in into the container itself
    - Access container (`bash`): `$ docker exec -it my-jupyter bash`
    - Run following command: `$ jupyter notebook list`
    - You will get result like this below:
        ```
        root@1cf8bb66ca39:/src/notebooks# jupyter notebook list
        Currently running servers:
        http://0.0.0.0:8888/?token=8750501cab033e0713e88505b2fe32b7da4d712cc469c638 :: /src/notebooks
        ```
    - Copy the token, e.g., `8750501cab033e0713e88505b2fe32b7da4d712cc469c638` and use it to login on the web-based jupyter
 6. Enjoy!
 
 ## Other important resource
 - **Install NVIDIA-Docker2**
    - Install dep: `$ sudo apt-get install apt-transport-https ca-certificates curl software-properties-common`
    - Setting the GPG and remote repo for the package:
        ```
        $ curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
            sudo apt-key add -
            distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
      
        $ curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
            sudo tee /etc/apt/sources.list.d/nvidia-docker.list
        ```
    - Update again: `$ sudo apt-get update
    - Install nvidia-docker (2) and reload the Docker daemon configurations:
        ```
        $ sudo apt-get install -y nvidia-docker2
        $ sudo pkill -SIGHUP dockerd
        ```
    - Test: `$ docker run --runtime=nvidia --rm nvidia/cuda:9.0-base nvidia-smi`