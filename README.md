## docker命令简介

### 镜像

1.查看镜像

`docker images`

2.删除镜像

`docker rmi $hash`

### 容器

1.查看容器

`docker ps [-a]`

2.停止容器

`docker stop $hash/$name`

3.删除容器

`docker rm $hash/$name`

4.运行容器

`docker run -it $images_name /bin/bash`

-it是前台运行

-d是后台运行

5.查看日志

`docker logs name`