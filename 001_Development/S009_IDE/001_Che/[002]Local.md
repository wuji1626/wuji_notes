#2 本地环境搭建
以下介绍通过Docker在本地搭建、运行、管理Che服务器的过程的方法
##2.1 获取帮助
在安装过程中发现的问题可以在：<br/>
https://github.com/eclipse/che/issues根据以下命令的输出到https://github.com/eclipse/che/blob/master/CONTRIBUTING.md上跟进问题<br/>
`docker run eclipse/che info`<br/>
`docker run eclipse/che info –bundle`<br/>
可以参考如下文档：<br/>
https://github.com/codenvy/che-docs/pulls<br/>
https://github.com/codenvy/che-docs/issues<br/>

\# Interactive help  
`sudo docker run -it eclipse/che start`  
\# Or, full start syntax where <path> is a local directory  
`sudo docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock -v <path>:/data eclipse/che start`


eg.  
`sudo docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock -v /var/tmp:/data eclipse/che start`

docker run eclipse/che stop







