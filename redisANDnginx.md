redis:
一、安装yum：
1）yum install  gcc-c++
2)上传redis解压包，上传方法有很多，比如用yum的rz上传工具，首先安装rz 执行：yum install  -y lrzsz
然后执行上传rz命令
3）解压redis压缩包到c/usr/local目录下面，tar -xvf redis...... -C/usr/local/
4）到redis3.0.。。。目录下，执行make命令执行编译
5）安装redis：执行命令make PREFIX=/usr/local/redis install命令
6）进入redis 进入bin 打开redis-server（./redis-server）这时服务就起来了
7）试验一下起没起来，新开一个连接，进去之后cd /usr/local/redis/bin 执行./redis-cli然后就会有一个等待输入的指令，输set username zhangsan 就会把zhangsan存进去，然后get username 就把zhangsan 取出来了

8)将redis启动改成后端启动与关闭：回到本目录，执行Ctrl+c停止运行，然后进redis3.0.0目录，将redis。conf拷贝到redis/bin下（cp redis.conf ../redis/bin）然后vim redis.conf 将里面的一行改为yes  然后 ./redis-server redis.conf后台启动  最后 ./redis-cli shutdown关闭掉
9)开启和关闭6379端口，centos7之后的相关方法：http://www.jb51.net/article/101576.htm

二、redis存值方法与相关指令：
1）get username   :"zhangsan "拿到key为username的值
incr username  :为username的值自增1，如果无法转成int则报错
incrby username 7：为username 的值增加7，如果无法转成int报错
相反，decr 与decrby为减
append username tianjin   :"zhangsantianjin"在value后面添加该值，没有相关key则创建
2）list:List中可以包含的最大元素数量是 4294967295。
lpush key 1 2 3 4 5,依次将数据从头部存入，没有相关key就创建一个
rpush  尾部
lrange key 0-1,从头到尾获取数据，-1表示到第一，-2倒数第二个
lpushx key value 在头部添加rpushx 尾部


nginx:
1. nginx安装
下载nginx：
 
官方网站：
http://nginx.org/
使用的版本是1.8.0版本。

 
Nginx提供的源码。
1.1. 要求的安装环境
1、需要安装gcc的环境。yum install gcc-c++
2、第三方的开发包。
n PCRE
PCRE(Perl Compatible Regular Expressions)是一个Perl库，包括 perl 兼容的正则表达式库。nginx的http模块使用pcre来解析正则表达式，所以需要在linux上安装pcre库。
yum install -y pcre pcre-devel
注：pcre-devel是使用pcre开发的一个二次开发库。nginx也需要此库。
n zlib
zlib库提供了很多种压缩和解压缩的方式，nginx使用zlib对http包的内容进行gzip，所以需要在linux上安装zlib库。
yum install -y zlib zlib-devel
 
n openssl
OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它目的使用。
nginx不仅支持http协议，还支持https（即在ssl协议上传输http），所以需要在linux安装openssl库。
yum install -y openssl openssl-devel
 
1.2. 安装步骤
 
第一步：把nginx的源码包上传到linux系统
第二步：解压缩
[root@localhost ~]# tar zxf nginx-1.8.0.tar.gz
第三步：使用configure命令创建一makeFile文件。
./configure \
--prefix=/usr/local/nginx \
--pid-path=/var/run/nginx/nginx.pid \
--lock-path=/var/lock/nginx.lock \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module \
--http-client-body-temp-path=/var/temp/nginx/client \
--http-proxy-temp-path=/var/temp/nginx/proxy \
--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
--http-scgi-temp-path=/var/temp/nginx/scgi
注意：启动nginx之前，上边将临时文件目录指定为/var/temp/nginx，需要在/var下创建temp及nginx目录
[root@localhost sbin]# mkdir /var/temp/nginx/client -p
第四步：make
第五步：make install

 
 
1.3. 启动nginx
进入sbin目录
[root@localhost sbin]# ./nginx

 
关闭nginx：
[root@localhost sbin]# ./nginx -s stop
推荐使用：
[root@localhost sbin]# ./nginx -s quit
 
重启nginx：
1、先关闭后启动。
2、刷新配置文件：
[root@localhost sbin]# ./nginx -s reload
 
1.4. 访问nginx

 
默认是80端口。
注意：是否关闭防火墙。



























