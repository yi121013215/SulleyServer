#个人Nginx搭建

###设计思路
宿主机搭建Nginx，然后通过宿主机搭建Docker，并且运行5个linux容器，分别为1个静态资源服务器、2个应用A服务器、2个应用B服务器。
>###静态资源服务器  
>域名：resource.sulley.cn  
>内网IP：172.19.0.2

>###应用A服务器  
>域名：album.sulley.cn  
>内网IP：172.19.1.1、172.19.1.2

>###应用B服务器  
>域名：blog.sulley.cn  
>内网IP：172.19.2.1、172.19.2.2


###环境搭建
1、docker搭建		sudo wget -qO- https://get.docker.com | sh  
2、拉取linux镜像  
3、镜像下载tomcat	wget https://mirrors.cnnic.cn/apache/tomcat/tomcat-8/v8.5.37/bin/apache-tomcat-8.5.37-fulldocs.tar.gz  
4、搭建java环境	yum -y install java-1.8.0-openjdk.x86_64  
5、docker运行容器因为使用的网络默认是bride，导致IP不固定，为了避免后续启动容器IP改变的情况，需要在容器启动的时候固定IP。所以需要创建自定义网络，具体操作如下

```
代码1
1. docker network create --subnet=172.19.0.0/24 resource_network
2. docker network create --subnet=172.19.1.0/24 blog_network
3. docker network create --subnet=172.19.2.0/24 ablum_network
4. docker run -it --name resource_sulley --network resource_network --ip 172.19.0.2 tomcat:resource /bin/bash
5. docker exec -it ContainerName /bin/bash 
6. docker restart ContainerId
```
