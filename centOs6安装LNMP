# centOs6 安装LNMP 

## mysql
1.

```
sudo yum install epel-release
```

2.

```
sudo yum update
```

3.	
	

```
sudo yum install mysql-server
```

4.
	

```
sudo service mysqld start
```

5.
	

```
sudo /usr/bin/mysql_secure_installation
```

6.
	

```
At the prompt "Enter current password for root (enter for none)", press Enter.
```

7.
	

```
The prompt will request the answer to a number of questions. 
	Y Y Y Y Y
```

8.	
	

```
sudo chkconfig mysqld on
```

9.	
	

```
sudo service mysqld restart
```

## nginx
1：安装必要的库

```
yum install gc gcc gcc-c++ pcre-devel zlib-devel openssl-devel
```

2：创建Nginx用户和组

```
groupadd www
#创建一个用户，不允许登陆和不创主目录 
useradd -s /sbin/nologin -g www -M www
```
3：下载并解压Nginx

```
wget http://nginx.org/download/nginx-1.12.0.tar.gz
tar -xzvf nginx-1.12.0.tar.gz
cd nginx-1.12.0
```
4：配置并编译安装

```
./configure --user=www --group=www --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-http_gzip_static_module
make && make install
```
参数说明：

nginx大部分常用模块，编译时./configure --help以--without开头的都默认安装。

--prefix=PATH ： 指定nginx的安装目录。默认 /usr/local/nginx

--conf-path=PATH ： 设置nginx.conf配置文件的路径。nginx允许使用不同的配置文件启动，通过命令行中的-c选项。默认为prefix/conf/nginx.conf

--user=name： 设置nginx工作进程的用户。安装完成后，可以随时在nginx.conf配置文件更改user指令。默认的用户名是nobody。--group=name类似

--with-pcre ： 设置PCRE库的源码路径，如果已通过yum方式安装，使用--with-pcre自动找到库文件。使用--with-pcre=PATH时，需要从PCRE网站下载pcre库的源码（版本4.4 - 8.30）并解压，剩下的就交给Nginx的./configure和make来完成。perl正则表达式使用在location指令和 ngx_http_rewrite_module模块中。

--with-zlib=PATH ： 指定 zlib（版本1.1.3 - 1.2.5）的源码解压目录。在默认就启用的网络传输压缩模块ngx_http_gzip_module时需要使用zlib 。

--with-http_ssl_module ： 使用https协议模块。默认情况下，该模块没有被构建。前提是openssl与openssl-devel已安装

--with-http_stub_status_module ： 用来监控 Nginx 的当前状态

--with-http_realip_module ： 通过这个模块允许我们改变客户端请求头中客户端IP地址值(例如X-Real-IP 或 X-Forwarded-For)，意义在于能够使得后台服务器记录原始客户端的IP地址

--add-module=PATH ： 添加第三方外部模块，如nginx-sticky-module-ng或缓存模块。每次添加新的模块都要重新编译（Tengine可以在新加入module时无需重新编译）

5：配置Nginx命令和服务并开机启动

```
vim /etc/init.d/nginx
```
复制一下代码到上面的文件

```
#!/bin/bash  
# nginx Startup script for the Nginx HTTP Server  
#  
# chkconfig: - 85 15  
# description: Nginx is a high-performance web and proxy server.  
# It has a lot of features, but it's not for everyone.  
# processname: nginx  
# pidfile: /var/run/nginx.pid  
# config: /usr/local/nginx/conf/nginx.conf  
nginxd=/usr/local/nginx/sbin/nginx  
nginx_config=/usr/local/nginx/conf/nginx.conf  
nginx_pid=/usr/local/nginx/nginx.pid  
 
RETVAL=0  
prog="nginx" 
 
# Source function library.  
. /etc/rc.d/init.d/functions  
 
# Source networking configuration.  
. /etc/sysconfig/network  
 
# Check that networking is up.  
[ ${NETWORKING} = "no" ] && exit 0  
 
[ -x $nginxd ] || exit 0  
 
 
# Start nginx daemons functions.  
start() {  
 
if [ -e $nginx_pid ];then 
   echo "nginx already running...." 
   exit 1  
fi  
 
   echo -n $"Starting $prog: " 
   daemon $nginxd -c ${nginx_config}  
   RETVAL=$?  
   echo  
   [ $RETVAL = 0 ] && touch /var/lock/subsys/nginx  
   return $RETVAL  
 
}  
 
 
# Stop nginx daemons functions.  
stop() {  
        echo -n $"Stopping $prog: " 
        killproc $nginxd  
        RETVAL=$?  
        echo  
        [ $RETVAL = 0 ] && rm -f /var/lock/subsys/nginx /var/run/nginx.pid  
}  
 
 
# reload nginx service functions.  
reload() {  
 
    echo -n $"Reloading $prog: " 
 $nginxd -s reload  
    #if your nginx version is below 0.8, please use this command: "kill -HUP `cat ${nginx_pid}`" 
    RETVAL=$?  
    echo  
 
}  
 
# See how we were called.  
case "$1" in 
start)  
        start  
        ;;  
 
stop)  
        stop  
        ;;  
 
reload)  
        reload  
        ;;  
 
restart)  
        stop  
        start  
        ;;  
 
status)  
        status $prog  
        RETVAL=$?  
        ;;  
*)  
        echo $"Usage: $prog {start|stop|restart|reload|status|help}" 
        exit 1  
esac  
 
exit $RETVAL
```


```
cd /etc/rc.d/init.d
#附加执行权限
chmod 755 /etc/init.d/nginx
#开机自启
chkconfig --level 345 nginx on
service nginx start #可选  start | stop | restart | reload | status |  help
```