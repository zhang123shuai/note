# 一、arm服务器docker离线部署tomcat

## 1.基础环境搭建（存放位置，项目文件代理位置等）

创建存放tomcat位置

```
#进入tomcat文件
cd /usr/local/tomcat/  

#若没有tomcat文件，创建
mkdir tomcat   

#进入
cd tomcat
```

使用xftp传入tomcat arm 镜像包 （付百度网盘地址）
链接：https://pan.baidu.com/s/14cvJN2M935WjotODnQlKww?pwd=6v08 
提取码：6v08

创建部署文件代理位置（此文件夹存放要部署的前端项目war包）

```
mkdir webapps    
```

## 2.上传tomcat镜像到docker

```
#加载镜像
docker load -i 文件名字
```

## 3.启动tomcat容器

### 3.1 查看docker网络模式是否有bridge网络

```
docker network ls
```

如果没有，将之前的docker network 清理后，重新create network后，容器全部重启

```
docker network create -d bridge vm-bf
```

### 3.2 启动并配置项目文件代理

查看所有镜像，找到tomcat镜像id

```
#查看所有镜像
docker images

#启动
docker run -di --name=tomcat -p 8088:8080 --network=vm-bf -v /usr/local/tomcat/webapps:/usr/local/tomcat/webapps tomcat:jdk8-armv8
```

至此配置完成，外部浏览器 主机:端口号访问。

注：docker run -di --name=镜像名字 -p 映射端口:内部启动端口 --network=bridge网络模式名字  -v 宿主机映射文件位置：docker->tomcat下的要映射的文件位置 镜像名字:TAG

# 二、项目打包成war包

在vscode中npm run build命令打包成dist文件，进入dist文件里执行      jar -cvf dist.war *    命令即可。

# 三、遇到的问题

## 1.访问页面404

```
#首先进入容器内
docker exec -it 你运行中的容器名或id /bin/bash

#已经进入了tomcat，然后进入webapps目录
cd webapps

#查看webapps里的文件，发现没有
ls

#退回到上一级
cd ..

#查看tomcat里的目录
ls

#发现有个webapps.dist，查看后发现有需要的文件，然后将webapps.dist里的文件复制到webapps中就可以了
cp -r webapps.dist/* webapps
```

# 四、其他操作命令

```
#退出容器
exit

#删除文件
rm -rf 文件名

#文件重命名
mv 旧文件名 新文件名

#停止指定容器
docker stop	容器id

#启动指定容器
docker start 容器id或者名字

#删除指定容器
docker rm 容器id

#删除镜像
docker rmi
```

# 五、参考文档

## 1.修改配置/etc/profile文件

https://blog.csdn.net/lezaitianyali/article/details/108879897

## 2.配置开启容器以及映射

https://blog.csdn.net/jerry11112/article/details/106170057

## 3.本地cmd命令打war包、解压war包

https://www.csdn.net/tags/NtTaYgysOTY1OTctYmxvZwO0O0OO0O0O.html

## 4.docker run 端口映射失败且无报错

https://blog.csdn.net/weixin_43705457/article/details/122434422

## 5.模拟Docker为容器建立bridge网络

https://blog.csdn.net/weixin_44864347/article/details/123460372

## 6.docker端口映射不生效

https://www.cnblogs.com/wanghaokun/p/16010850.html