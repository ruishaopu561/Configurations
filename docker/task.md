## Docker 发布 Spring Boot 应用
### 打包  
得到文件名类似```wordladder-0.0.1-SNAPSHOT.jar```
### 上传
将jar包和```Dockerfile```放在同级目录里，```Dockerfile```内容
```
FROM java:8
MAINTAINER rsp
ADD wordladder-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","/app.jar"]
```
解释：
```
1，基础镜像java 版本是8
2,作者rsp
3,重命名为app.jar
4,监听8080
5，启动运行 java -jar app.jar
```
### 编辑镜像
```
docker build -t ruishaopu561/wordladder .
```
通过```docker images```查看镜像
### 运行
```
docker run -d --name docker -p 8080:8080 ruishaopu561/wordladder
```
### 常用命令
#### 删除容器id
```
docker rm 容器id
```
#### 删除镜像  镜像id
```
docker rmi image-id
```
#### 查看容器日志
```
docker logs container-name /container-id
```
#### 导入导出容器
```
docker export CONTAINER(容器) > 地址文件名
[root@my10 songlj]# docker save d11c3799fa6a > /home/songlj/java8.tar
docker import - 地址文件名
docker import - /home/songlj/java8.tar
```
#### 保存/加载
```
docker save IMAGE(镜像) > 地址文件名
docker save 9610cfc68e8d > /home/songlj/java8.tar
docker load < 地址文件名
docker load < /home/songlj/java8.tar
```
#### 停止容器
```
$ docker ps // 查看所有正在运行容器
$ docker stop containerId // containerId 是容器的ID
```
```
$ docker ps -a // 查看所有容器
$ docker ps -a -q // 查看所有容器ID
```
```
$ docker stop $(docker ps -a -q) //  stop停止所有容器
$ docker  rm $(docker ps -a -q) //   remove删除所有容器
```
## 遇到的问题
### 容器名称冲突
运行时报的错
```
docker: Error response from daemon: Conflict. The container name "/docker" is already in use by container "2552d901aa8148e811a48a413756081b94a98380349411e97e74ae8649a87f7a". You have to remove (or rename) that container to be able to reuse that name.
```
解决
确认它是否确实存在：
```
$ docker ps -l
```
上面命令输出中的最后一列是名称。

如果容器存在，请使用以下命令删除它：
```
$ docker rm qgis-desktop-2-4
```
或强行使用，
```
$ docker rm -f qgis-desktop-2-4
```
用ID删除也可以
### commit错误
```
docker build -t <username>/<repo>:<tag> .
```
```
docker commit -a 'rsp' -m 'update' ID ruishaopu561/wordladder:v1
```
Then you can push
```
docker push <username>/<repo>:<tag>
```
## Reference
[Docker发布应用](https://www.jianshu.com/p/d05642c32929)
