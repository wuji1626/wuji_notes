[TOC]

##1.环境准备和配置
服务器	|ip	|操作系统	|备注|
---|---|----|----
tomcat-001|192.168.199.215|Centos7|	
tomcat-001|192.168.199.216|Centos7| 
nginx-001|192.168.199.217|Centos7| 	
nginx-002|192.168.199.218|Centos7|  

##2.安装Centos7
- 安装操作系统
- 安装ntp

##3.安装tomcat7
- 通过yum 安装 tomcat7
- 通过tar.gz文件来安装

##4.安装nginx
- 通过yum 安装 nginx
- 本机编译安装nginx

##5.安装tomcat 测试程序(nginx动静分离和tomcat集群)
开发测试页
~~~jsp
<%@ page language="java" %>
<html>
  <head><title>TomcatA</title></head>
  <body>
    <h1><font color="red">TomcatA </font></h1>
    <table align="centre" border="1">
      <tr>
        <td>Session ID</td>
    <% session.setAttribute("abc","abc"); %>
        <td><%= session.getId() %></td>
      </tr>
      <tr>
        <td>Created on</td>
        <td><%= session.getCreationTime() %></td>
     </tr>
    </table>
  </body>
</html>
~~~
将war包放置到tomcat的webapp目录下  
运行tomcat，让单机tomcat都正常运行  

##6.配置nginx，实现tomcat负载均衡
vim /etc/nginx/nginx.conf
~~~
#修改events的块内容
events {
    use epoll;
    worker_connections  2048;
}
#在http块里面增加如下内容
    #开启zip网页压缩
    gzip  on;
    gzip_min_length 1k;
    gzip_buffers 4 8k;
    gzip_http_version 1.1;
    gzip_types text/plain application/x-javascript text/css application/xml;

    #反向代理
    ## meerkat_web
    upstream backserver {
            #ip_hash; #每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题
            server 192.168.199.215:8080 weight=1 max_fails=2 fail_timeout=30s;
            server 192.168.199.216:8080 weight=2 max_fails=2 fail_timeout=30s;
    }
# 在server块里面的location部分替换成如下内容
            location / {
                root   html;
                index  index.html index.htm index.jsp;
                proxy_pass  http://meerkat_web;
                proxy_set_header Host  $http_host;
                proxy_set_header Cookie $http_cookie;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
                client_max_body_size  100m;
            }   
~~~
nginx可以根据客户端IP进行负载均衡，在upstream里设置ip_hash，就可以针对同一个C类地址段中的客户端选择同一个后端服务器，除非那个后端服务器宕了才会换一个。

nginx的upstream目前支持的5种方式的分配


1、轮询（默认） 
每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。 
upstream backserver { 
server 192.168.0.14; 
server 192.168.0.15; 
} 

2、指定权重 
指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。 
upstream backserver { 
server 192.168.0.14 weight=10; 
server 192.168.0.15 weight=10; 
} 

3、IP绑定 ip_hash 
每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。 
upstream backserver { 
ip_hash; 
server 192.168.0.14:88; 
server 192.168.0.15:80; 
} 

4、fair（第三方） 
按后端服务器的响应时间来分配请求，响应时间短的优先分配。 
upstream backserver { 
server server1; 
server server2; 
fair; 
} 

5、url_hash（第三方） 
按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。 
upstream backserver { 
server squid1:3128; 
server squid2:3128; 
hash $request_uri; 
hash_method crc32; 
} 

在需要使用负载均衡的server中增加 

proxy_pass http://backserver/; 
upstream backserver{ 

ip_hash; 
server 127.0.0.1:9090 down; (down 表示单前的server暂时不参与负载) 
server 127.0.0.1:8080 weight=2; (weight 默认为1.weight越大，负载的权重就越大) 
server 127.0.0.1:6060; 
server 127.0.0.1:7070 backup; (其它所有的非backup机器down或者忙的时候，请求backup机器) 
} 

max_fails ：允许请求失败的次数默认为1.当超过最大次数时，返回proxy_next_upstream 模块定义的错误 
  
fail_timeout:max_fails次失败后，暂停的时间

##7.测试
- 测试自动切换 
访问http://192.168.1.217/ 代表的nginx服务器，会出现index.jsp显示的效果。这个时候如果关闭对应的tomcat服务，就会自动切换到另外一台上。
- 测试负载均衡 
访问http://192.168.1.217/ 代表的nginx服务器，会出现index.jsp显示的效果。但是多次刷新后，一直显示的是同一台tomcat服务。说明负载均衡没有实现。解决的办法是把upstream里面的 ip_hash 这个给屏蔽掉。就会自动切换了。

##8.实现Session共享
利用Tomcat自身session复制，修改Server.xml
- Membership中的bind：本机地址
- Membership的address：组播地址，用于广播。需要注意IP地址要符合组播地址的规则，集群中的各节点组播地址要相同
- Membership的frequency：session同步的频次，需要注意太频繁损失性能，太不频繁session信息不能及时同步
- Receiver的port：用于接收组播中的session同步信息，集群中每个Tomcat的端口都不能相同
- 其他配置都可以参照如下配置

~~~xml
<Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"
channelSendOptions="8">

<Manager className="org.apache.catalina.ha.session.DeltaManager"
expireSessionsOnShutdown="false"
notifyListenersOnReplication="true"/>

<Channel className="org.apache.catalina.tribes.group.GroupChannel">
<Membership className="org.apache.catalina.tribes.membership.McastService"
bind="127.0.0.1"
address="228.0.0.4"<!--保留ip,用于广播-->
port="45564"
frequency="500"
dropTime="3000"/> 
<Receiver className="org.apache.catalina.tribes.transport.nio.NioReceiver"
address="auto"
port="4001"<!--如果是在同一台机器上的两个tomcat做负载，则此端口则不能重复-->
autoBind="100"
selectorTimeout="5000"
maxThreads="6"/>
<Sender className="org.apache.catalina.tribes.transport.ReplicationTransmitter">
<Transport className="org.apache.catalina.tribes.transport.nio.PooledParallelSender"/>
</Sender>
<Interceptor className="org.apache.catalina.tribes.group.interceptors.TcpFailureDetector"/>
<Interceptor className="org.apache.catalina.tribes.group.interceptors.MessageDispatch15Interceptor"/>
</Channel>

<Valve className="org.apache.catalina.ha.tcp.ReplicationValve" filter=""/>
<Valve className="org.apache.catalina.ha.session.JvmRouteBinderValve"/>

<ClusterListener className="org.apache.catalina.ha.session.JvmRouteSessionIDBinderListener"/>
<ClusterListener className="org.apache.catalina.ha.session.ClusterSessionListener"/>
</Cluster>
~~~

注意：在工程下的web.xml中增加<distributable/>  

