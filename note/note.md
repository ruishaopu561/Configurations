## 安装anaconda3
不知道怎么回事，安装最新版的anaconda3完成后却找不到任何东西，
```
# 将anaconda的bin目录加入PATH，根据版本不同，也可能是~/anaconda3/bin
echo 'export PATH="~/anaconda3/bin:$PATH"' >> ~/.bashrc
# 更新bashrc以立即生效
source ~/.bashrc
```
terminal运行上述两行命令之后就可以了
## 安装配置ssr
安装ssr
```
sudo apt install shadowsocks
```
配置设置
```
vim /etc/shadowsocks.json
```
贴入以下代码
```
{
"server":"你的ip地址",
"server_port":端口号,
"local_port":1080,
"password":"你的秘玛",
"timeout":600,
"method":"加密方式"
}
```
运行指令：
```
sslocal -c /etc/shadowsocks.json
```
这样本地服务启动了，这时去配置一下代理，推荐去系统设置配置。
![]()
## 终端美化
终端安装命令:
```
sudo apt-get install zsh
sudo apt-get install git
sudo wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
chsh -s /bin/zsh
```
看看你有哪些主题
```
ll ~/.oh-my-zsh/themes/
```
在```~/.zshrc```中修改```ZSH_THEME="bira"```，双引号处即为主题名。
### Failed to receive SOCKS4 connect request ack.
原因：根据error，我ss客户端的代理用的是socket5，git的https使用的是socket4, 设置git的代理为socket5就好了。
于是：
```
git config --global http.proxy 'socks5://127.0.0.1:1080' 
```
http处直接按tap补全（我执行时无https选项），设置完立刻生效。
## Reference
- [ubuntu16.04下anaconda3的安装和配置](https://blog.csdn.net/white__hacker/article/details/81066971)  
- [Linux也可以这样美](https://www.jianshu.com/p/f9e905abea91)
- [Ubuntu 16.04 LTS深度美化](https://www.jianshu.com/p/4bd2d9b1af41)
- [Failed to receive SOCKS4 connect request ack. 解决方法](https://blog.csdn.net/baidu_36482169/article/details/82818490)
