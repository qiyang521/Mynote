#### Docker笔记

  1. 安装:	

     (1) win7或win8下载安装地址 :	http://mirrors.aliyun.com/docker-toolbox/windows/docker-toolbox/

     (2)win10 安装说明:

     ​		1. 组合键Ctrl+Shift+Esc---->任务管理器页面中点击<性能>----->查看虚拟化是否启用----->如果没有启用			虚拟化,需要重启电脑,Dell电脑示例(按下F2 进入BIOS,进入高级模块设置,找到 Intel Virtualization 			Technology，设置为Enabled,开启支持虚拟化)

     ​		2.此电脑----->右键<属性>----->控制面板主页----->程序和功能------->启动或关闭wingdows功能--->Hyper-		V下,点击勾选"Hyper-V管理工具"和"Hyper-V平台",然后根据提示,需要重启.

     ​		3.下载地址: https://www.docker.com/get-started 

     ​		4.镜像加速: 在Docker Engine中的registry-mirrors加入如下:		

     ```python
         {
           "registry-mirrors": [
             "https://registry.docker-cn.com",
             "http://hub-mirror.c.163.com",
             "https://docker.mirrors.ustc.edu.cn",
             "https://cr.console.aliyun.com/"
           ],
           "insecure-registries": [],
           "debug": true,
           "experimental": false
         }
     ```

2. docker常用命令:
   (1). win10 docker镜像导入导出:

   ```python
   # 下载镜像,例如mysql,splash
   	docker pull mysql,docker pull scraping/splash
   # 查看镜像
   	docker images
   # 导出镜像
   	docker save xxx(镜像id) -o f:\xxx\xxx\mysql.tar(路径)	
   # 导入镜像
   	docker load -i f:\xxx\xxx\mysql.tar 
       docker import http:\xxx\xxx(url)或者f:\xxx\xxx\mysql.tar 	
   # 镜像重命名
   	docker tag xxx(镜像id) mysql:8.0(新的命名)       
   # 导出容器快照:
   	docker export xxx(容器id) > f:\xxx\xxx\mysql.tar(路径)  
   # 导入容器快照:
   	docker import - f:\xxx\xxx\mysql.tar
   # 删除镜像:
   	docker rmi xxx(镜像id)
   ```

   (2). win10 查看容器的IP:

   ​		1.docker exec -it xxx(容器id) bash

   ​		2.cat /etc/hosts 查看容器的IP

   (3). win10 docker基础命令:sj

   ```python
   # 启动一个容器:启动服务实例:
   	实例1:docker run -name splash_n -p 8050:8050 -d scrapinghub/splasrangwh
   	实例2:docker run -name nginx_n -p 80:80 -d nginx
       实例3:docker run -d -P nginx
       # 说明: 实例1,2:-name 后面自定义容器名称,:前表示的是提供外围访问的端口(可以自定义),:后是一个内部		容器开放的网络端口,比如mysql默认端口3306,splash默认端口8050,nginx默认端口80;实例3中,随机分		分配一个端口到内部容器开放的网络端口
       
   # 启动一个容器: 一般情况下-i和-t组合使用-it
   	docker run -it ubuntu(镜像) /bin/bash		# 其中-i:交互式操作,-t:终端,/bin/bash:交互式shell
   # 启动已停止运行的容器
   	docker ps -a
   # 后台运行: -d
   	docker run -itd --name ubuntu-test ubuntu /bin/bash
   # 停止,启动,重启
   	docker stop/start/restart xxx(容器id/容器名)
   # 进入容器的两种方式:
   	docker attach 和 docker exec	# 推荐使用 docker exec 命令，因为此退出容器终端，不会导致容器的停止
       docker attach 1e560fca3906(容器id)	# 当使用exit时,容器也随之停止并退出.
       docker exec -it 243c32535da7(容器id) /bin/bash	# 当使用exit退出时,该容器依然不会停止,并继续运行.
   # 删除单个/批量容器
   	docker rm -f 容器1 容器2 容器3...
   # 删除所有已经停止的容器
   	docker container prune
   ```

   (4). win10 docker启动mysql时,使用navicat 连接时报错解决方案:
   
   ```python
   # 后台运行mysql容器
   	docker run -name mysql_test -p 3307:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql
   # 进入mysql容器
   	docker exec -it mysql_test bash
   # 访问MySQL服务:
   	mysql -h localhost -uroot -p
   # host为 % 表示不限制ip   localhost表示本机使用
   	alter user 'root'@'%' identified with mysql_native_password by '123456';
       flush privileges;
       ## 连接数据库成功 ##
   ```
   

3. Dockerfile

   1. 使用docker Dockerfile 构建运行scrapyd服务

      (1) 新建一个目录,比如docker_pro

      (2) 在目录下新建三个文件,Dockerfile,scrapyd.conf,requirements.txt

      (3) 三个文件说明:

      ​	Dockerfile: 构建自定义镜像时需要的配置,配置信息如下:

      ```python
      	FROM python:3.6	
          ADD .  /code	
          WORKDIR /code
          COPY ./scrapyd.conf /etc/scrapyd/
          EXPOSE 6800
          RUN pip --default-timeout=100 install -r requirement.txt
          CMD scrapyd
          # 第一行的FROM是指在python:3.6这个镜像上构建，也就是说在构建时就已经有了Python 3.6的环境。
          # 第二行的ADD是将本地的代码放置到虚拟容器中。它有两个参数：第一个参数是. ，即代表本地当前路径；		第二个参数/code代表虚拟容器中的路径，也就是将本地项目所有内容放置到虚拟容器的/code目录下。
          # 第三行的WORKDIR是指定工作目录，这里将刚才添加的代码路径设成工作路径，这个路径下的目录结构和当	前本地目录结构是相同的，所以在这个目录下可以直接执行库安装命令。
          # 第四行的COPY是将当前目录下的scrapyd.conf文件复制到虚拟容器的/etc/scrapyd/目录下，		Scrapyd在运行的时候会默认读取这个配置。
          # 第五行的EXPOSE是声明运行时容器提供服务端口，注意这里只是一个声明，运行时不一定会在此端口开启		服务。这个声明的作用，一是告诉使用者这个镜像服务的运行端口，以方便配置映射，二是在运行使用随机端	口映射时，容器会自动随机映射EXPOSE的端口。
          # 第六行的RUN是执行某些命令，一般做一些环境准备工作。由于Docker虚拟容器内只有Python3环境，而		没有Python库，所以我们运行此命令来在虚拟容器中安装相应的Python库，这样项目部署到Scrapyd中便可	以正常运行。
          # 第七行的CMD是容器启动命令，容器运行时，此命令会被执行。这里我们直接用scrapyd来启动Scrapyd服	务。
          
      ```

   ​	  (4)  docker build -t scrapyd:latest  .  或  docker build -t scrapyd:v1.0  .(指定自己构建的版本)

   ​	  (5)  构建完成后启动测试:

   ​				docker run -d -p 6800:6800 scrapyd



















