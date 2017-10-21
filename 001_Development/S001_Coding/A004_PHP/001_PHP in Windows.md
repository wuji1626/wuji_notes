
##1 PHP安装与配置
1. 下载php-5.2.16-nts-Win32-VC6-x86.zip
[php-5.2.16-nts-Win32-VC6-x86.zip](http://windows.php.net/downloads/releases/archives/php-5.2.16-nts-Win32-VC6-x86.zip)  
2. 将PHP解压到如下路径：  
`D:\dev\PHP\php-5.2.16`  
3. 修改php.ini  
将php.ini-recommended重命名为php.ini  
修改配置：`extension_dir = "./"`为`D:\dev\PHP\php-5.2.16\ext`  
释放：
>;extension=php_mysql.dll
;extension=php_mysqli.dll
>;cgi.fix_pathinfo=1
4. 修改Apache配置httpd.conf  
修改本地apache server地址：
`ServerRoot "D:/dev/Middleware/apache/Apache24"`
添加：  
~~~
# php5 support
LoadModule php5_module D:/dev/PHP/php-5.6.31/php5apache2_4.dll
AddType application/x-httpd-php .php .html .htm
# configure the path to php.ini
PHPIniDir "D:/dev/PHP/php-5.6.31"
~~~
将DocumentRoot修改为实际web工程存放位置  
DocumentRoot "D:/www"
<Directory "D:/www">
IfModule下增加index.php
<IfModule dir_module>
    DirectoryIndex index.php index.html
</IfModule>

##2 运行
用管理员运行cmd，执行如下命令，将Apache发布成服务  
`httpd.exe -k install -n "Apache24"`  
服务卸载
`httpd.exe -k uninstall -n "Apache24"`  

##QA
1. 如有要运行memadmin，PHP需要增加memcached组件php_memcache.dll，下载与php版本匹配的dll，放置到php的ext目录下  
在php.ini中添加
`extension=php_memcache.dll`
2. Apache端口号修改：  
`Listen 80`


