# 1. 部署说明

## python优势

```python
"""
1.一门面向对象的语言
2.拥有丰富的库
3.可移植性
4.免费\开源
5.简单易学
"""
可以做软件开发,人工智能,web开发等等
```



# 2.部署流程

centos7.5+Nginx+python+Django+uwsgi(把python变成一个服务)+mysql

## 安装Nginx

```python
# yum -y install wget   安装wget
# wget http://nginx.org/download/nginx-1.15.5.tar.gz   下载
# yum -y install pcre-devel zlib-devel   安装依赖
# ./configure --prefix=/usr/local/nginx   检测环境
# make
# make install
# /usr/local/nginx/sbin/nginx  启动nginx服务
# elinks http://192.168.10.42 -dump   打开测试页
# rm -rf nginx-1.15.5      删除源码
```

## 安装python

```python
# wget https://www.python.org/ftp/python/3.7.1/Python-3.7.1.tar.xz  下载
# tar xf Python-3.7.1.tar.xz   解压
# cd Python-3.7.1   进入目录
# yum -y install gcc-*openssl-*libffi-devel sqlite-devel    安装依赖
# ./configure --enable-optimizations --with-openssl=/usr/bin/openssl   配置环境检测
# make -j8  安装优化
# make install  安装，默认安装路径： /usr/local/lib/python3.7
```

### 升级pip

```python
# pip3 install --upgrade pip
```

### 报错解决

```python
# vim Python-3.7.1/Modules/Setup  编辑文件
# set nu     显示行号
"""
把：
	211 SSL=/usr/local/ssl
	212 _ssl _ssl.c \
	213 	-DUSE_SSL -I$(SSL)/include -I$(SSL)/include/openssl \
	214 	-L$(SSL)/lib -lssl -lcrypto
打开（每行前的#去掉）
"""
# make -j8
# make install     重新安装下
```

### 安装和卸载ipython

```python
# pip3 install ipython    安装
# pip3 uninstall ipython    卸载
```

### 虚拟环境安装及使用

```python
# pip3 install virtualenv    安装
# virtualenv -p python3 web    创建虚拟环境项目目录
# source web/bin/activate    开起指定目录下的虚拟环境
```

### Django出现ALLOWED_HOSTS报错

```python
# vim settings.py   打开配置文件
"""
/ALLOWED   搜索ALLOWED_HOSTS位置
ALLOWED_HOSTS = ['*']  
"""
重启Django
```

## 安装mysql

### 安装依赖包

```python
# yum -y install ncurses-devel gcc-* bzip2-* bison
```

### 升级cmake工具

```python
# yum search cmake   查找是否有cmake
# wget https://cmake.org/files/v3.13/cmake-3.13.0-rc2.tar.gz
# tar xf cmake-3.6.0-rc1.tar
# cd cmake-3.6.0-rc1
# ./configure  配置
# make  -j4   编译  -j4的意思是4个核工作
# make install    安装
# cmake --version  测试
```

### 升级boost库文件

```python
"""
mysql-5.7对应的boost版本只有boost_1_59_0.tar.bz2
下载地址:  https://www.boost.org/users/history/version_1_59_0.html
"""
# tar xf boost_1_59_0.tar.bz2  解压
# mv boost_1_59_0 /usr/local/boost  移动文件到指定目录下
```

### 安装mysql

```python
# tar xf mysql-5.7.29.tar.gz
# cmake .\
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \   指定安装路径
-DMYSQL_DATADIR=/usr/local/mysql/data/ \    指定数据目录
-DMYSQL_UNIX_ADDR=/usr/local/mysql/mysql.sock \   指定sock文件路径
-DWITH_INNOBASE_STORAGE_ENGINE=1 \     安装innodb存储引擎
-DWITH_MYISAM_STAORAGE_ENGINE=1 \      安装myisam存储引擎
-DENABLED_LOCAL_INFILE=1 \       允许使用load data命令从本地导入数据
-DEXTRA_CHARSETS=all -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci \ 安装所有字符集\默认字符集utf-8\ 校验字符
-DMYSQL_USER=mysql \    mysql用户名
-DWITH_DEBUG=0 \  关闭debug
-DWITH_EMBEDDED_SERVER=1 \   生成一个libmysqld.a(.so)的库,这个库同时集成了mysql服务与客户端API
-DDOWNLOAD_BOOST=1 \     允许boost
-DENABLE_DOWNLOADS=1 \   允许下载boost库文件
-DWITH_BOOST=/usr/local/boost 
# make -j4
# make install

安装完成后操作
# cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysql   拷贝连接
# chmod 755 /etc/init.d/mysql  给予权限
# useradd -s /sbin/nologin -r mysql   创建用户
# chown mysql.mysql /usr/local/mysql/ -R  改变用户
# ll /usr/local/mysql   查看用户

创建常用连接到相应目录下
# ln -sf /usr/local/mysql/bin/* /usr/bin/
# ln -sf /usr/local/mysql/lib/* /usr/lib/
# ln -sf /usr/local/mysql/libexec/* /usr/local/libexec
# ln -sf /usr/local/mysql/share/man/man1/* /usr/share/man/man1
# ln -sf /usr/local/mysql/share/man/man8/* /usr/share/man/man8

修改配置文件
# vim /etc/my.cnf
"""
[mysqld]
basedir=/usr/local/mysql
datadir=/usr/local/mysql/data
socket=/usr/local/mysql/mysql.sock
[mysqld_safe]
log-error=/var/log/mysql.log
pid-file=/var/run/mysql.pid
"""

初始化数据库
"""
/usr/local/mysql/bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql/ --datadir=/usr/local/mysql/data/
初始化完成之后会有一个临时的密码在最后出现,需要记录下来
"""
```

### 启动测试

```python
# /etc/init.d/mysql start
# lsof -i :3306
```

### 密码修改

```python
# mysql_secure_installation
```

### 登录数据库

```python
# mysql -u root -p 密码
```

## 部署发布平台

### 请求处理流程

```python
"""
request
   |
nginx    ---->   静态文件直接处理
   |
WSGI or UWSGI server
   |
python web serveice
"""
```

### 安装uwsgi

```python
"""
	uwsgi是服务器和服务端应用程序的通信协议,规定了怎么把请求转发给应用程序和返回.
	uWSGI是一个Web服务器,它实现了WSGI协议\uwsgi\http等协议.Nginx中HttpUwsgiModule的作用是与uWSGI服务器进行交换.
	nginx和uWSGI交互就必须使用同一个协议,而上面说了uwsgi支持fastcgi,uwsgi,http协议,这些都是nginx支持的协议,只要大家沟通好使用哪个协议,就可以正常运行了.
"""
# pip3 install uwsgi
# mkdir /etc/uwsgi
# vim /etc/uwsgi/uwsgi.ini
"""
[uwsgi]
uid = root
gid = root
socket = 127.0.0.1:9090  // 监听的ip和端口
master = true     // 启动主线程
vhost = true      // 多站模式
no-site = true    // 多站模式时不设置入口模块和文件
workers = 2       // 子进程数量
reload-mercy = 10 // 平滑的重启
vacuum = true     // 退出重启时清理文件
max-requests = 1000  // 开启1000个进程后,自动respawn下
limit-as = 512    // 将进程的总内存量控制在512M
buffer-size = 30000
pidfile = /var/run/uwsgi9090.pid   // pid文件,用于下面的脚本启动\停止该进程
daemonize = /var/log/uwsgi9090.log
pythonpath = /root/web/lib/python3.7/site-pachkages   // 使用的python路径
"""
方法一:
# uwsgi --ini /etc/uwsgi/uwsgi.ini  启动
# netstat -ntpl
# cat /var/run/uwsgi9090.pid
# kill -9 pid号

方法二:
# vim /etc/init.d/uwsgi
"""
文件内容省略
PIDFILE=/var/run/${NAME}9090.pid
"""
# which uwsgi  查看uwsgi路径
# chmod 755 /etc/init.d/uwsgi
文件的使用
# /etc/init.d/uwsgi start
# /etc/init.d/uwsgi stop
# /etc/init.d/uwsgi status

# netstat -ntpl   查看端口使用情况

# vim /usr/local/nginx/conf/nginx.conf
"""

"""

先启动uwsgi,在启动nginx.
```



# 3. 发布web

# 4.测试

