# Docker Tips

###

### Basic Operation

```shell
# create new container from image
docker run [image name/id]
​
# remove images
docker rmi [image id]
​
# list all the containers
docker ps -a
​
# start container
docker start [container name/id]
​
# enter container
docker exec -it [container name/id] /bin/sh
​
# If you are using docker on Linux(like Ubuntu), the following command may be correct.
docker exec -it [container name/id] bash
​
# stop container
docker stop [container name/id]
​
# remove container
docker rm [container name/id]
​
# import local images
# e.g.: docker import - zsh < ~/Desktop/ctf_zsh.tar
docker import - [SHELL] < [path to local images]
​
# export container to local images
# e.g.: docker export 3b742508bae5 > ~/Desktop/ctf_zsh2.tar
docker export [container name/id] > ~ [path to local images]
​
# exit docker interface
exit   # Ctrl-d is also ok
```

### Mount host file system

```shell
# Create new contain and set up Docker Volume
# where 6666 can be replace by IMAGE ID 
# `pwd` means the current host path, you can also use your customed path.
# /home/oslab/ is the docker container path
# -u oslab denotes the user, which is optional. If omitted, root is the default user
docker run -it -v `pwd`:/home/oslab/ -u oslab -w /home/oslab 6666 /bin/bash
```

### Commit and push

```shell
# login
docker login
​
# check docker id and repo name
# make sure that REPOSITORY is your "dockerID/repoName"
docker images
​
# find the container id you want to commit
docker ps
​
# docker commit ==> this command will create a local image according to the container
# e.g.: docker commit -a "syk" -m "backup oracle" 21c9f82f8249 sykoracle:v1
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
​
# check the new image
# there will be a new image, e.g.: 
# sykoracle  v1  9c204c2b4fde   42 seconds ago   8.24GB
docker images
​
# change the image name
# docker tag 9c204c2b4fde 264119/sykoracle:v1
docker tag imageID dockerID/repoName:tagname
​
# push
# e.g: docker push 264119/sykoracle:v1
docker push dockerID/repoName:tagname
​
# now you can check it on your docker hub website
```

### Create image with conda env

```shell
conda env export > environment.yml
```

Create Dockerfile

```shell
FROM continuumio/anaconda3 
WORKDIR PATH_TO_WORKDIR
 
# Create the environment:
COPY environment.yml .
RUN conda env create -f environment.yml
 
SHELL ["/bin/bash", "-c"]
 
RUN echo "conda activate YOUR_ENV" > ~/.bashrc
ENV PATH /opt/conda/envs/env/bin:$PATH
 
CMD ["/bin/bash"]
```

```shell
sudo docker build -t imagename:tag .
```

#### Use docker commands in GitLab runner

```shell
sudo usermod -aG docker gitlab-runner
sudo -u gitlab-runner -H docker info
```

#### Run container with environment parameters

```shell
docker run -p 8080:8080 --name [container name] --env env1='env1' --env env2='env2' [image]:[version] python main.py
```

### Dockerfile&#x20;

Dockerfile is a blueprint for building the image

&#x20;![](.gitbook/assets/image.png)

&#x20;Dockerfile example:

(the name should always be `Dockerfile` )

```docker
FROM node:13-alpine

ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=password
    
RUN mkdir -p /home/app

COPY . /home/app

CMD ["node", "/home/app/server.js"]
```

How to build an image with Dockerfile

```shell
docker build -t my-app:1.0 .
```


### Proxy in Docker

```
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo touch /etc/systemd/system/docker.service.d/http-proxy.conf
```

Add the following content to `http-proxy.conf`:
```shell
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:<port>"   # use your port
Environment="HTTPS_PROXY=http://127.0.0.1:<port>"  # use your port
```

### Reference

[Docker Volume - 目录挂载以及文件共享](https://kebingzao.com/2019/02/25/docker-volume/)

[https://docs.gitlab.com/ee/ci/docker/using\_docker\_build.html](https://docs.gitlab.com/ee/ci/docker/using\_docker\_build.html)
