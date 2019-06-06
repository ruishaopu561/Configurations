# PyTorch
## NVIDIA
用```lspci | grep -i nvidia``查看GPU版本信息
## Anaconda3
[官网](https://www.anaconda.com/)下载```python3.7```版本，下载后按要求安装。

因为我的terminal做了优化，用的是zsh，所以遇到```zsh: command not found: conda```的问题时，要在```~/.zshrc```文件中加入
```
export PATH="/home/rsp/anaconda3/bin:$PATH"
```
然后
```
source ~/.zshrc
```
再次激活，就可以了。
## PyTorch
### Missing dependencies for SOCKS support
在PyCharm安装python模块(不仅仅是安装模块)出现如下错误，终端使用pip3命令也是同样的错误。这是因为本地设置了socks代理，但是python不支持socks代理，就会报错。
```
Could not install packages due to an EnvironmentError: Missing dependencies for SOCKS support.
```

《Python安装模块出现Missing dependencies for SOCKS support》

#### 解决方法
1.安装pysocks模块，配合socks代理工具使用。

Unset socks proxy
```
unset all_proxy
unset ALL_PROXY
```
Install missing dependencies
```
pip install pysocks
```
Reset proxy
```
source ~/.bashrc
```

## Reference
- [Linux查看GPU信息和使用情况](https://blog.csdn.net/dcrmg/article/details/78146797)  
- [zsh: command not found: conda](https://blog.csdn.net/codechelle/article/details/77414117)  
- [Python安装模块出现Missing dependencies for SOCKS support](https://blog.tearth.me/python_installmodule/)
