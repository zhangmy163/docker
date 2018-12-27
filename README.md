## docker命令简介

### 镜像

1.查看镜像

   `docker images`

2.删除镜像（镜像没有被使用）

   `docker rmi $IMAGE_ID`

### 容器
1.查看容器

   `docker ps -a`

2.停止容器

   `docker stop $NAMES/$CONTAINER_ID`

3.删除容器(容器没运行，如果运行状态可以先stop再rm，也可以rm -f)

   `docker rm [-f] $NAMES/$CONTAINER_ID`

4.运行容器（容器名称不能重复）

   `docker run -itd --name 容器名 镜像ID或镜像名称:tag 运行命令`

   例如：

   `docker run -it --name test1 centos bash`    #前台运行

   `docker run -d --name test2 centos bash`    #后台运行


   docker run还可以带其他参数-p端口映射，-v挂载磁盘，--name，--entrypoint等等，详细参考以下文档

   [docker run参数说明](http://www.runoob.com/docker/docker-run-command.html)   

6.查看日志

   `docker logs [-f] $NAMES/$CONTAINER_ID`

7.进入正在运行的容器

   `docker exec -it $NAMES/$CONTAINER_ID bash`
