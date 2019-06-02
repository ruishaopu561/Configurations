# 添加一个Ubuntu的开机启动服务
如果要添加为开机启动执行的脚本文件，可先将脚本复制或者软连接到```/etc/init.d/```目录下，然后用：
```
update-rc.d xxx defaults NN
```
命令(NN为启动顺序)，将脚本添加到初始化执行的队列中去。

1. 新建一个脚本文件 test.sh
```
#！/bin/bash
# command content
echo "hello start up script!" > /home/sc/Desktop/mystart.txt
exit 0
```
2. 将脚本放置到启动目录/etc/init.d下
```
sudo mv test.sh  /etc/init.d/
```
3. 设置脚本文件的权限
```
cd /etc/init.d/
sudo chmod 755 test.sh
```
4. 将脚本添加到启动脚本中
```
sudo update-rc.d  test.sh  defaults  90
```
其中数字90是脚本启动的顺序号，数字越大表示执行的越晚，按照自己的需要相应修改即可。  
重启后可以在桌面上看到生成的mystart.txt文件。

移除ubuntu开机脚本：
```
sudo update-rc.d -f test.sh remove
```
