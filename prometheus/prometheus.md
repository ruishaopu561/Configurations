# Prometheus

## Prometheus server

### 安装推荐
```shell
$ sudo docker run -d -p 9090:9090 prom/prometheus
```
### 运行
```shell
$ ./prometheus --config.file=prometheus.yml
```
也可以
```shell
$ sudo cp /{path of prometheus} /usr/local/bin
$ prometheus
```
然后在```http://localhost:9090/```打开

## Grafana

### 安装推荐
```shell
$ sudo docker run -d -p 3000:3000 grafana/grafana
```
安装完成之后使用```http://localhost:3000/```进入，默认```username/password```=```admin/admin```

之后的东西见参考吧，很详细。

## Grafana 重置admin密码

### 查找grafana.db文件

```
find / -name "grafana.db"
PS:一般默认文件为/var/lib/grafana/grafana.db
```
### 使用sqlite3加载数据库文件
```
sqlite3 /var/lib/grafana/grafana.db
#.tables查看有那些表
.tables
#select查看表里面的内容
select * from user;
#使用update更新密码
update user set password = 'new password', salt = 'F3FAxVm33R' where login = 'admin';
#修改完成后退出
.exit
```

## Reference
[实战Prometheus搭建监控系统](https://www.aneasystone.com/archives/2018/11/prometheus-in-action.html)