# Docker基础概念

* docker image 镜像：docker虚拟机磁盘静态存储，相当于virtualbox的虚拟硬盘，Dockerfile编译生成镜像，就是生成完成特定业务功能文件系统创建与保存的过程，生成后的镜像可以在容器中运行。
* docker container 容器：docker image的运行时状态，启动运行容器时必须指定执行一个命令，完成后容器将立即停止退出。利用这一特性，可以用容器来运行可持续提供的服务让容器保持运行，比如nginx。也可以使用alias别名调用容器中的命令，运行完立即销毁，做干净的环境隔离，比如php composer命令，nodejs的npm命令。
* Dockerfile：单一镜像生成配置文件，使用特定语法写明构建镜像规则，比如：指定基础包是基于ubuntu还是centos还是alpine，指定预装哪些应用程序，指定初始配置文件的内容，指定开放的端口和目录，指定镜像被放入容器运行时需要执行的命令等。
* docker-compose.yml：一组容器编译/运行配置文件，控制一组容器联合协作，共同完成特定业务的服务组合，批量控制镜像的编译、容器的运行与销毁。

## Docker基础命令操作

* 列出镜像列表：docker image ls
* 列出运行中容器列表：docker ps
* 列出所有容器列表：docker ps -a
* 停止单一容器运行：docker stop container_name 或 docker stop container_id
* 删除单一镜像：docker image rm image_name 或 docker image rm image_id
* 删除单一容器：docker rm container_name 或 docker rm container_id


* 启动一个镜像进入shell不退出：docker run -it image_name sh
* 进入容器shell不退出：docker exec -it container_name sh
* 启动一个镜像执行一个命令并退出：docker run --rm image_name command_name
* 在容器中执行一个命令并退出：docker exec container_name command_name

* 批量删除所有镜像：docker image rm $(docker images -q)
* 批量停止所有容器：docker stop $(docker ps -aq)
* 批量删除所有容器：docker rm $(docker ps -aq)

## docker-compose 相关
> docker-compose 命令 需要在`docker-compose.yml`文件所在目录执行。`.env`或`docker-compose.yml`或`Dockerfile`文件更改后都需要重新创建相应服务的镜像。在 `docker-compose build`完之后，使用`docker-compose up`命令应用新创建的镜像。


* 批量创建镜像：docker-compose build
* 批量启动容器：docker-compose up -d
* 不使用缓存创建某个镜像：docker-compose build --no-cache service_name
* 重启某个服务（这个命令不会应用新创建的镜像） docker-compose restart service_name
