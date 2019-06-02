# Note in ML
## virtualenv
virtualenv就是用来为一个应用创建一套“隔离”的Python运行环境。

### 安装virtualenv
使用pip安装virtualenv
```
pip install virtualenv
```
### virtualenv基本用法
然后，假定我们要开发一个新的项目，需要一套独立的Python运行环境，可以这么做： 
1. 首先新建项目文件夹
```
mkdir myproject
cd myproject/
```
2. 创建一个独立的Python运行环境，命名为venv
```
virtualenv --no-site-packages venv
```
命令```virtualenv```就可以创建一个独立的Python运行环境

参数```–no-site-packages```，已经安装到系统Python环境中的所有第三方包都不会复制过来，这样，我们就得到了一个不带任何第三方包的“干净”的Python运行环境  
参数 ```–system-site-packages```，虚拟环境会继承```/usr/lib/python2.7/site-packages```下的所有库。如果想依赖系统包目录，则可以使用此功能。如果您想要与全局系统隔离，请不要使用此参数。  

3. 激活虚拟环境
新建的Python环境被放到当前目录下的venv目录。有了venv这个Python环境，可以用```source```进入该环境：
```
$ source venv/bin/activate
(venv)$
```
注意到命令提示符变了，有个(venv)前缀，表示当前环境是一个名为```venv```的Python环境。 
下面正常安装各种第三方包，并运行python命令：
```
(venv)$ pip install jinja2
```
在venv环境下，用pip安装的包都被安装到venv这个环境下，系统Python环境不受任何影响。也就是说，venv环境是专门针对myproject这个应用创建的。

关闭虚拟环境
退出当前的venv环境，使用deactivate命令：
```shell
$ deactivate 
```
此时就回到了正常的环境，现在pip或python均是在系统Python环境下执行。

### 指定python版本
可以使用```-p PYTHON_EXE```选项在创建虚拟环境的时候指定python版本
```zsh
Mac:myproject johnson$ virtualenv -p /usr/bin/python2.7 venv
```
到此已经可以解决python版本冲突问题和python库不同版本的问题
### Reference
- [virtualenv配置python虚拟环境](https://blog.csdn.net/John_xyz/article/details/80665320)
