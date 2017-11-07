#Docker常用命令
docker常用的命令有如下分类：  
- 容器生命周期管理 — docker [run|start|stop|restart|kill|rm|pause|unpause]
- 容器操作运维 — docker [ps|inspect|top|attach|events|logs|wait|export|port]
- 容器rootfs命令 — docker [commit|cp|diff]
- 镜像仓库 — docker [login|pull|push|search]
- 本地镜像管理 — docker [images|rmi|tag|build|history|save|import]
- 其他命令 — docker [info|version]

##1 列出机器上的镜像（images）
docker images 

##2 在docker index中搜索image（search）
docker search TERM

##3 从docker registry server 中下拉image或repository（pull）
docker pull [OPTIONS] NAME[:TAG]

##4 推送一个image或repository到registry（push）
docker push seanlook/mongo  
docker push registry.tp-link.net:5000/mongo:2014-10-27  

##5 从image启动一个container（run）
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
eg：
docker run -i -t --name mytest centos:centos6 /bin/bash  
--name：参数可以指定启动后的容器名字  

-p参数：  
- -p 11211:11211 这个即是默认情况下，绑定主机所有网卡（0.0.0.0）的11211端口到容器的11211端口上
- -p 127.0.0.1:11211:11211 只绑定localhost这个接口的11211端口
- -p 127.0.0.1::5000
- -p 127.0.0.1:80:8080

-v参数：
-v [host_path:container_path]  

docker run --name nginx_test \
 -v /tmp/docker:/usr/share/nginx/html:ro \
 -p 80:80 -d \
 nginx:1.7.6  
在主机的/tmp/docker下建立index.html，就可以通过http://localhost:80/或http://host-ip:80访问了。  

##6 将一个container固化为一个新的image（commit）
docker commit <container> [repo:tag]  

##7 开启/停止/重启container（start/stop/restart）
docker start/stop/restart CONTAINER_ID  

##8 连接到正在运行中的container（attach）
docker attach --sig-proxy=false $CONTAINER_ID  
--sig-proxy=false 确保CTRL-D或CTRL-C不会关闭容器  

##9 查看image或container的底层信息（inspect） 
docker inspect --format='{{.NetworkSettings.IPAddress}}' $CONTAINER_ID  

##10 删除一个或多个container、image（rm、rmi）
docker rm <container_id/contaner_name>  
rm：删除停止的容器  
docker rmi <container_id/contaner_name>  
rmi：可以删除运行中的容器  

##11 给镜像打上标签（tag）
将同一IMAGE_ID的所有tag，合并为一个新的  
docker tag 195eb90b5349 seanlook/ubuntu:rm_test  
新建一个tag，保留旧的那条记录  
docker tag Registry/Repos:Tag New_Registry/New_Repos:New_Tag  

##12 查看容器的信息container（ps）
docker ps命令可以查看容器的CONTAINER ID、NAME、IMAGE NAME、端口开启及绑定、容器启动后执行的COMMNAD。经常通过ps来找到CONTAINER_ID。  
docker ps 默认显示当前正在运行中的container  
docker ps -a 查看包括已经停止的所有容器  
docker ps -l 显示最新启动的一个容器（包括已停止的）  

##13 查看容器中正在运行的进程（top）
容器运行时不一定有/bin/bash终端来交互执行top命令，查看container中正在运行的进程，况且还不一定有top命令，这是docker top <container_id/container_name>就很有用了。实际上在host上使用ps -ef|grep docker也可以看到一组类似的进程信息，把container里的进程看成是host上启动docker的子进程就对了。  

##14 其他命令
docker还有一些如login、cp、logs、export、import、load、kill等不是很常用的命令  



