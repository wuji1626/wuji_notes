#2 本地环境搭建
以下介绍通过Docker在本地搭建、运行、管理Che服务器的过程的方法
##2.1 获取帮助
在安装过程中发现的问题可以在：  
https://github.com/eclipse/che/issues  根据以下命令的输出到      
https://github.com/eclipse/che/blob/master/CONTRIBUTING.md  上跟进问题   
`docker run eclipse/che info`  
`docker run eclipse/che info –bundle`  
可以参考如下文档：<br/>
https://github.com/codenvy/che-docs/pulls<br/>
https://github.com/codenvy/che-docs/issues<br/>

##2.2 Che Docker镜像获取并运行
\# Interactive help  
`sudo docker run -it eclipse/che start`  
\# Or, full start syntax where <path> is a local directory  
`sudo docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock -v <path>:/data eclipse/che start`


在本地可以利用如下命令访问  
`sudo docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock -v /var/tmp:/data eclipse/che start`  
![](./images/13.png)

关闭镜像实例使用如下命令：  
docker run eclipse/che stop

##2.3 通过浏览器访问本地Che
访问以下url，即可访问本地的Che IDE      
http://192.168.119.132:8080/









