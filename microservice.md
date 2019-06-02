# MicroService

## consul
### Install
```
cd /usr/local/bin/
wget https://releases.hashicorp.com/consul/1.0.0/consul_1.0.0_linux_amd64.zip?_ga=2.86515800.1546778999.1510551854-179199314.1510551854
unzip consul_1.0.0_linux_amd64.zip
```
### run
```/usr/local/bin```路径下
```
./consul agent -dev
```
或任意路径下
```
consul agent -dev
```
consul运行状态下输入```consul members```查看状态（两个终端操作）
输入```http://127.0.0.1:8500```访问Consul，可查看到默认的ui界面：

## 未解决
consul远端访问

## Reference
[下载Consul](https://www.consul.io/downloads.html)
[consul常用命令](https://blog.csdn.net/Coder_501/article/details/79352911)
[微服务详细介绍](https://www.jianshu.com/p/572f264d0bb5)
