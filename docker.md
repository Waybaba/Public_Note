# **DOCKER**

## Function

Create a virtual environment/situation in which you can run some specific applications such as python, apache ... without disturb the whole devices. All the services can be managed by the Docker. 

For more detail tutors: <https://docs.docker.com/get-started/part2/>

## Differences between docker and virtual machine

![1556528196766](C:\Users\Way\iCloudDrive\Note\Public_Note\Public_Note\Data\1556528196766.png)

The access of virtual machine to host resources is MADE by the running system, while the container can get access to the resources in a less cost way.

> A **container** runs *natively* on Linux and shares the kernel of the host machine with other containers. It runs a discrete process, taking no more memory than any other executable, making it lightweight.
>
> By contrast, a **virtual machine** (VM) runs a full-blown “guest” operating system with *virtual* access to host resources through a hypervisor. In general, VMs provide an environment with more resources than most applications need.

## Basic concept

**Container** can be spawned by the **image**.

**Image** is just like package that can be installed. Every time we run an image, a new **container** is born. So that **container** can be treated as an application.

## Installation

The installation of docker in different system have some small differences. Basically, I follow the half-official tutors [here](https://yeasy.gitbooks.io/docker_practice/install/ubuntu.html). 

Tips: It seems that the docker-ce is not in the traditional apt-get source and the adding way of the tutor is wrong. I use this when installing it on Raspberry-Pi 3B+:

```bash
echo "deb [arch=armhf] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/raspbian/ \
     $(lsb_release -cs) stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list
```

I met some problems when following the half-official tutors such as "GPIO balabala" when running apt-get. Finally, I get to follow [a tutor in Chinese](https://segmentfault.com/a/1190000017109351) and set the root source with the Tsinghua source. More details are in the tutor. 

In this part, I mainly focus on the installation on **Ubuntu18.04** instead.

1. refresh index and upgrade system.

   ```bash
   sudo apt update
   sudo apt upgrade
   ```

2. install dependencies.

   ```bash
   sudo apt install apt-transport-https ca-certificates curl software-properties-common
   ```

3. install the key for docker repo # this is special.

   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```

4. add Docker APT software repository to system list.

   ```bash
   sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
   ```

5. test

   ```bash
   sudo systemctl status docker
   
   # the successful output will be like this:
   ● docker.service - Docker Application Container Engine
     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2018-06-18 01:22:00 PDT; 6min ago
       Docs: https://docs.docker.com
   Main PID: 10647 (dockerd)
       Tasks: 21
     CGroup: /system.slice/docker.service
   ```

   



## Basic 

### Install a hello-world image

Hello-world image is very small so that they can be download and run in a very short time. When we run it successfully, the container will send back a simple message.

1. download/pull a image

   ```
   docker pull hello-world
   ```

2. docker run hello-world

   ```bash
   docker run hello-world
   ```

### Useful command

- show general state

  ```bash
  docker info
  ```

- detail state about containers.

  ```bash
  docker ps --all # adding "--all" will include the existing ones like "hello-world" 
  docker container ls --all # -aq: show all in quiet mode
  ```

- detail state about images

  ```
  docker iamge ls
  ```

  

docker --version : test

docker info : see state

### Permission problem

The docker daemon is owned by the root user, so the docker command need to be preface with sudo. Creating a Unix group called docker and add users to it can fix it. When the Docker daemon starts, it create a Unix socket accessible by members of t he docker group.

>1. Create the `docker` group.
>
>   ```bash
>   $ sudo groupadd docker
>   ```
>
>2. Add your user to the `docker` group.
>
>   ```bash
>   $ sudo usermod -aG docker $USER
>   ```
>
>3. Log out and log back in so that your group membership is re-evaluated.
>
>   If testing on a virtual machine, it may be necessary to restart the virtual machine for changes to take effect.
>
>   On a desktop Linux environment such as X Windows, log out of your session completely and then log back in.
>
>4. Verify that you can run `docker` commands without `sudo`.
>
>   ```bash
>   $ docker run hello-world
>   ```

## Second

The second part is about how to DIY a environment, while this is not useful for me. The more common way to use docker is pulling the existing environment from the [Dockhub](https://hub.docker.com/r/ufoym/deepo).

```bash
# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

## Dockerhub

In this part, I try to install a very popular image from dockerhub, which is called [ufoym/deepo](ufoym/deepo). It contains almost all the modules for deep learning. It is worth mentioning that all the dependencies are resolved automatically.

### Grammar of pulling

```bash
# simple pulling
docker pull ufoym/deepo 
# from specific source
docker pull docker.mirrors.ustc.edu.cn/ufoym/deepo
# pull the image of specific tag (the default one is latest)
docker pull docker.mirrors.ustc.edu.cn/ufoym/deepo:all-py36
```





​	