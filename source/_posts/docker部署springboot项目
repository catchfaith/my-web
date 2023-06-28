---
title: docker部署springboot项目
date: 2022-06-28 10:54:05
categories: Java
---

## 1.准备工作
### 1.1 新创建一个springboot项目
![Test](/images/docker01.png)

对项目进行打包 打包成功后复制jar包到服务器
### 1.2 编写Dockerfile
***注意这里创建dockerfile文件名必须是Dockerfile***
``` txt
FROM openjdk:8
VOLUME /tmp
#ADD 后面的参数是项目名字 / 后面的参数是自定义的别名
ADD java-excel-demo-1.0-SNAPSHOT.jar /dockertest.jar
#这里的最后一个变量需要和前面起的别名相同
#EXPOSE 8088
ENTRYPOINT ["java","-jar","/dockertest.jar"]
```
>VOLUME 指定临时文件目录为/tmp。在主机 /var/lib/docker 目录下创建临时文件，挂载到容器的/tmp。/tmp目录用来将容器内数据持久化到外部主机，因为Spring Boot内嵌的Tomcat默认工作目录为/tmp。

文件准备完毕
![Test](/images/docker02.png)
### 1.3 构建镜像
``` bash
docker build -t javaweb .
```

### 1.4 运行容器
``` bash
[root@120 javaweb]# docker run -d -p 8088:8088 -it -v /root/docker/javaweb/log/info:/log/info -v /root/docker/javaweb/log/warn:/log/warn -v /root/docker/javaweb/log/error:/log/error --name springtest javaweb
```
这里我挂载了log目录的文件夹

### 1.5 测试接口是否正常
``` bash
[root@120 javaweb]# curl http://localhost:8088/api/excel/test
docker success!
```