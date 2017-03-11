#Oracle安装

[TOC]


##1 CentOS操作系统安装Oracle
###1.1 安装环境及软件
Linux服务器： CentOS6.5     64位  
Oracle软件版本：Oracle11gR2  
###1.2 系统要求
Linux安装Oracle系统要求：  

| 系统要求 | 说明 |
|--------|--------|
| 内存 | 必须高于1G的物理内存（可用free命令查看Mem 项） |
| 交换空间 | 一般为内存的2倍，例如：1G的内存可以设置swap 分区为3G大小（可用free命令查看Swap 项） |
| 硬盘 | 5G以上 |

###1.3 修改操作系统核心参数及关闭防火墙
在Root用户下执行以下步骤：  
1）修改用户的SHELL的限制，修改/etc/security/limits.conf文件  
输入命令：vi /etc/security/limits.conf，按i键进入编辑模式，将下列内容加入该文件。  
oracle   soft    nproc    2047  
oracle   hard    nproc    16384  
oracle   soft    nofile    1024  
oracle   hard    nofile    65536  

2）修改/etc/pam.d/login 文件，输入命令：vi    /etc/pam.d/login，按i键进入编辑模式，将下列内容加入该文件。  
session   required    /lib/security/pam_limits.so  
session   required    pam_limits.so  

3）修改linux内核，修改/etc/sysctl.conf文件，输入命令: vi  /etc/sysctl.conf ，按i键进入编辑模式，将下列内容加入该文件  
fs.file-max = 6815744  
fs.aio-max-nr = 1048576  
kernel.shmall = 2097152  
kernel.shmmax = 2147483648  
kernel.shmmni = 4096  
kernel.sem = 250 32000 100 128  
net.ipv4.ip_local_port_range = 9000 65500  
net.core.rmem_default = 4194304  
net.core.rmem_max = 4194304  
net.core.wmem_default = 262144  
net.core.wmem_max = 1048576  

4）使 /etc/sysctl.conf 更改立即生效，执行以下命令。 输入：sysctl  -p 显示如下：
~~~shell
[root@cdh-node4 hadoop]# sysctl -p
net.ipv4.ip_forward = 0
net.ipv4.conf.default.rp_filter = 1
net.ipv4.conf.default.accept_source_route = 0
kernel.sysrq = 0
kernel.core_uses_pid = 1
net.ipv4.tcp_syncookies = 1
error: "net.bridge.bridge-nf-call-ip6tables" is an unknown key
error: "net.bridge.bridge-nf-call-iptables" is an unknown key
error: "net.bridge.bridge-nf-call-arptables" is an unknown key
kernel.msgmnb = 65536
kernel.msgmax = 65536
kernel.shmmax = 68719476736
fs.file-max = 6815744
fs.aio-max-nr = 1048576
kernel.shmall = 2097152
kernel.shmmax = 2147483648
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 4194304
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
~~~

5）创建相关用户和组，作为软件安装和支持组的拥有者。
创建用户，输入命令：
groupadd  oinstall 
groupadd  dba
创建Oracle用户和密码,输入命令：
useradd -g oinstall -g dba -m oracle
passwd  oracle
然后会让你输入密码，密码任意输入2次，但必须保持一致，回车确认（输入oracle作为密码）

6）禁止linux安全需修改”vi /etc/selinux/config”文件，修改如下：
SELINUX=disabled
并执行systemctl stop iptables
如果操作系统不支持systemd,请执行:
service iptables stop

7）关闭firewalld防火墙，并禁止其自动启动【fedora特有】。
systemctl stop firewalld
systemctl enable firewalld.service

8）创建oracle安装目录
mkdir -p /u01/app/oracle/product/11.2.0/db_1
chown -R oracle:oinstall /u01
chmod -R 775 /u01

9）修改主机名妓IP地址
#修改主机名
[root@oracledb ~]# sed -i "s/HOSTNAME=localhost.localdomain/HOSTNAME=cdh-node4/" /etc/sysconfig/network
[root@oracledb ~]# hostname cdh-node4
#添加主机名与IP对应记录
[root@oracledb ~]# vi /etc/hosts
192.168.196.224     cdh-node4    

10）修改Linux系统发行信息
首先备份原有文件（因为安装完oracle之后，还需要替换回来）
cp /etc/redhat-release /etc/redhat-release.bak
然后使用vi /etc/redhat-release ,将其内容修改为：
redhat release 5

11）预先安装oracle11g需要的依赖包
    如果未联网，需要配置yum本地库

yum install –y binutils
yum install –y compat-libstdc++-33
yum install –y compat-libstdc++-33.i686
yum install –y elfutils-libelf
yum install –y elfutils-libelf-devel
yum install –y gcc
yum install –y gcc-c++
yum install –y glibc
yum install –y glibc.i686
yum install –y glibc-common
yum install –y glibc-devel
yum install –y glibc-headers
yum install –y glibc-devel.i686
yum install –y ksh
yum install –y libaio
yum install –y libaio.i686
yum install –y libaio-devel003_OR~1.MD
yum install –y libaio-devel.i686
yum install –y libgcc
yum install –y libgcc.i686
yum install –y libstdc++
yum install –y libstdc++.i686
yum install –y libstdc++-devel
yum install –y make
yum install –y numactl
yum install –y numactl-devel
yum install –y sysstat
yum install –y unixODBC
yum install –y unixODBC.i686
yum install –y unixODBC-devel
yum install –y unixODBC-devel.i686
yum install –y zip
yum install –y pdksh
安装pdksh时会出错
[root@localhost usr]# yum install –y pdksh
Loaded plugins: fastestmirror, refresh-packagekit, security
Loading mirror speeds from cached hostfile
 * base: mirrors.yun-idc.com
 * extras: mirrors.yun-idc.com
 * updates: mirrors.yun-idc.com
Setting up Install Process
No package pdksh available.
Error: Nothing to do

手动下载:  
@[pdksh-5.2.14-1.i386.rpm](./attach/pdksh-5.2.14-1.i386.rpm)  

手动安装
rpm -ivh pdksh-5.2.14-36.el5.i386.rpm 
仍然出错：
warning: pdksh-5.2.14-36.el5.i386.rpm: Header V3 DSA/SHA1 Signature, key ID e8562897: NOKEY
error: Failed dependencies:
pdksh conflicts with ksh-20100621-2.el6.i686
解决：
rpm -e ksh-20120801-10.el6_5.11.x86_64
rpm -ivh pdksh-5.2.14-1.i386.rpm  

###1.4 修改oracle用户环境变量
在oracle用户下执行以下步骤：
1）新建临时目录     mkdir /home/oracle/tmp
2）使用oracle用户登录，编辑vi .bash_profile配置文件，添加如下内容：
~~~shell
export TMP=/home/oracle/tmp
export TMPDIR=$TMP
export ORACLE_HOSTNAME=centos65
export ORACLE_UNQNAM=DB11G
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=$ORACLE_BASE/product/11.2.0/db_1
export ORACLE_HOME_LISTNER=$ORACLE_HOME
export ORACLE_SID=orcl
export ORACLE_TERM=xterm
export PATH=/usr/sbin:$PATH
export PATH=$ORACLE_HOME/bin:$PATH
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib:$LD_LIBRARY_PATH
export CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib:$CLASSPATH
 
if [ $USER = "oracle" ]; then
  if [ $SHELL = "/bin/ksh" ]; then
    ulimit -p 16384
    ulimit -n 65536
  else
    ulimit -u 16384 -n 65536
  fi
fi
~~~

###1.5 开始安装Oracle软件：
1）使用oracle用户登录ftp软件，将oracle11g的两个安装文件上传到/home/oracle目录下。
2）在图形界面以oracle用户登陆，并打开一个运行终端（在fedora18中可使用ctrl + shift + t 快捷方式）。
3）在运行终端运行unzip命令解压oracle安装文件，命令如下：

~~~shell
cd  /home/oracle
unzip  linux.x64_11gR2_database_1of2.zip 
unzip  linux.x64_11gR2_database_2of2.zip
~~~

4）在运行终端中执行如下安装命令：
~~~shell
as root :
chown -R oracle /home/oracle/database
chmod -R 750 /home/oracle/database
su  oracle
cd  /home/oracle/database
./runInstaller
~~~

之后会启动oracle 安装界面。
如果出现host +的问题，可以考虑用oracle用户登录系统，从图形界面中运行runInstaller  
![](./images/I01.png)  

5）安装过程
·选择不进行Oracle的support  
![](./images/I02.png)  
·提示没有指定email，关键安全事件时没有紧急联系邮箱，忽略直接Yes  
![](./images/I03.png)  
·忽略软件升级：  
![](./images/I04.png)  
·选择安装数据库并配置一个实例  
![](./images/I05.png)  
·选择安装Server版  
![](./images/I06.png)  
·选择单节点  
![](./images/I07.png)  
·选择高级安装  
![](./images/I08.png)  
·选择语言  
![](./images/I09.png)  
·选择企业版  
![](./images/I10.png)  
·选择数据库的安装路径：  
![](./images/I11.png)  
·选择oracle的库  
![](./images/I12.png)  
·选择OLTP服务  
![](./images/I13.png)  
·选择SID  
![](./images/I14.png)  
·内存选择9G  
![](./images/I15.png)  
·选择字符集  
![](./images/I16.png)  
·选择安全设定  
![](./images/I17.png)  
·不选择建立schema  
![](./images/I18.png)  
·管理端设定  
![](./images/I19.png)  
·选择数据文件存储  
![](./images/I20.png)  
·不进行自动备份  
![](./images/I21.png)  
·设置SYS、SYSTEM、SYSMAN、DBSNMP用户密码：password  
![](./images/I22.png)  
由于密码简单会提示一个警告，可以忽略  
![](./images/I23.png)  
·操作系统组设定，使用默认  
![](./images/I24.png)  
·进行Oracle安装时的Check  
![](./images/I25.png)  
·Check结果，选择忽略警告，进行下一步  
![](./images/I26.png)  
  ■解决pdksh-5.2.14问题：  
  1）下载pdksh-5.2.14-37.el5.x86_64.rpm   
  2）rpm -ivh pdksh-5.2.14-37.el5.x86_64.rpm 时总出错误：  
~~~shell
	error: Failed dependencies:  
    pdksh conflicts with ksh-20120801-10.el6.x86_64  
~~~
3）采用强制安装：rpm -ivh --nodeps pdksh-5.2.14-37.el5.x86_64.rpm 
  ■解决Fixable错误可以执行：
~~~shell
    cd /home/oracle/tmp/CVU_11.2.0.4.0_oracle/  
    ./runfixup.sh  
~~~
·开始安装  
  ■安装过程中出现：  
![](./images/I27.png)  
·安装成功：  
![](./images/I28.png)  
2 配置数据库  
在oracle用户的图形界面中，新开启一个终端，输入命令dbca来创建数据库。  
![](./images/I29.png)  
1）创建数据库  
![](./images/I30.png)  
2）选择一般目的  
![](./images/I31.png)  
3）SID  
![](./images/I32.png)  
4）企业Manager设置去掉  
![](./images/I33.png)  
5）SYS、SYSTEM密码：password  
![](./images/I34.png)  
6）选择文件系统  
![](./images/I35.png)  
7）不选择Fast Recovery  
![](./images/I36.png)  
8）不选择schema  
![](./images/I37.png)  
9）内存  
![](./images/I38.png)  
10）连设为150  
![](./images/I39.png)  
11）字符集UTF8  
![](./images/I40.png)    
12）连接模式  
![](./images/I41.png)  
13）存储  
![](./images/I42.png)  
14）创建数据库  
![](./images/I43.png)  
15）确认  
![](./images/I44.png)  
16）安装配置数据库  
![](./images/I45.png)  

##2 Ubuntu操作系统安装Oracle
1）确保按JDK  
在服务器上安装jdk1.8.121  
2）安装Oracle所需要的依赖包  
在安装过程中出现：  
![](./images/I46.png)  
可以在命令后直接执行`sudo apt-get -f install`  
~~~
sudo apt-get install automake  
sudo apt-get install autotools-dev  
sudo apt-get install binutils  
sudo apt-get install bzip2  
sudo apt-get install elfutils  
sudo apt-get install expat  
sudo apt-get install gawk  
sudo apt-get install gcc  
sudo apt-get install gcc-multilib  
sudo apt-get install g++-multilib  
sudo apt-get install ia32-libs  
(ia32-libs已经被替换：可以使用sudo apt-get install lib32ncurses5 lib32z1)  
sudo apt-get install ksh  
sudo apt-get install less  
sudo apt-get install lesstif2  
（E: Unable to locate package lesstif2）  
sudo apt-get install lesstif2-dev
（E: Package 'lesstif2-dev' has no installation candidate）  
sudo apt-get install lib32z1  
sudo apt-get install libaio1  
sudo apt-get install libaio-dev  
sudo apt-get install libc6-dev  
sudo apt-get install libc6-dev-i386  
sudo apt-get install libc6-i386  
sudo apt-get install libelf-dev  
sudo apt-get install libltdl-dev  
sudo apt-get install libmotif4  
（由libxm4:i386 libuil4:i386 libmrm4:i386 libxm4 libuil4 libmrm4 libmotif-common替代）  
sudo apt-get install libodbcinstq4-1 libodbcinstq4-1:i386  
sudo apt-get install libpth-dev  
sudo apt-get install libpthread-stubs0  
（E: Unable to locate package libpthread-stubs0）
sudo apt-get install libpthread-stubs0-dev  
sudo apt-get install libstdc++5  
sudo apt-get install lsb-cxx  
（E: Unable to locate package lsb-cxx）
sudo apt-get install make  
sudo apt-get install openssh-server
sudo apt-get install pdksh  
（E: Package 'pdksh' has no installation candidate）  
sudo apt-get install rlwrap  
sudo apt-get install rpm  
sudo apt-get install sysstat  
sudo apt-get install unixodbc  
sudo apt-get install unixodbc-dev  
sudo apt-get install unzip  
sudo apt-get install x11-utils  
sudo apt-get install zlibc
~~~
在执行sudo apt-get install automake 时出现
`E: dpkg was interrupted, you must manually run 'sudo dpkg --configure -a' to correct the problem.`



