# 一、Linux-nginx离线部署

## 1.准备工作

在Linux桌面创建nginxOfflinePack文件夹；把所需要的安装包放在此文件夹里；并进入此文件夹 

```apl
cd nginxOfflinePack
```

## 2.开始安装环境

```apl
rpm -Uvh *.rpm --nodeps --force
```

## 3.安装pcre、openssl、zlib

```apl
tar -zxvf pcre-8.34.tar.gz
cd pcre-8.34
./configure
make
make install

tar -zxvf openssl-1.0.1u.tar.gz
cd openssl-1.0.1u
./configure或者./config
make
make install

tar -zxvf zlib-1.2.11.tar.gz
cd zlib-1.2.11
./configure
make
make install
```

## 4.安装nginx

```apl
tar -zxvf nginx-1.16.1.tar.gz
cd nginx-1.16.1
./configure
make
make install
```

**注意**:执行make install后 nginx的安装目录会打印出来，不指定的话默认都是该目录

/usr/local/nginx     /usr/local/nginx/sbin

### ①初次启动

cd /usr/local/nginx/sbin

**执行指令** ./nginx

### ②停止nginx

cd /usr/local/nginx/sbin

执行指令./nginx -s stop

### ③重启nginx

cd /usr/local/nginx/sbin

**执行指令**./nginx -s reload

### ④查看nginx是够启动

ps -ef|grep nginx

# 二、Linux安装nginx并设置开机自启（命令行方式）

## 1.设置开机自启

首先修改/etc/rc.d/rc.local文件，具体步骤如下：

```apl
vim /etc/rc.d/rc.local
```

（输入键盘“ i ”进入可编辑模式，esc退出编辑模式，:wq保存并退出。）

添加如下内容：

```api
/usr/local/nginx/sbin/nginx
```

执行以下命令，使/etc/rc.d/rc.local变成可执行文件。

```apl
chmod +x /etc/rc.d/rc.local
```

使用reboot命令重启后，查看nginx是否成功的自启动了。

# 三、参考网址

CentOS7离线安装Nginx：https://www.cnblogs.com/sanluorenjian/p/13189999.html

Centos7系统离线安装nginx步骤：https://blog.csdn.net/qq445829096/article/details/106259672

Linux安装nginx并设置开机自启（命令行方式）：https://blog.csdn.net/fei1234456/article/details/107234075/

下载插件：http://rpmfind.net/linux/rpm2html/search.php