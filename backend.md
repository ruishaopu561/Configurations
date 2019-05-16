# Backend
记录搭建后端开发环境中，安装java,maven,tomcat,idea的过程。
## Java
Java的教程是个很好的教程，简洁明了。

To install this version, first update the package index:
```
sudo apt update
```
Next, check if Java is already installed:
```
java -version
```
If Java is not currently installed, you'll see the following output:
```
Command 'java' not found, but can be installed with:

apt install default-jre
apt install openjdk-11-jre-headless
apt install openjdk-8-jre-headless
apt install openjdk-9-jre-headless
Execute the following command to install OpenJDK:
```
Execute the following command to install OpenJDK:

```
sudo apt install default-jre
```
This command will install the Java Runtime Environment (JRE). This will allow you to run almost all Java software.

Verify the installation with:
```
java -version
```
You'll see the following output:
```
openjdk version "10.0.1" 2018-04-17
OpenJDK Runtime Environment (build 10.0.1+10-Ubuntu-3ubuntu1)
OpenJDK 64-Bit Server VM (build 10.0.1+10-Ubuntu-3ubuntu1, mixed mode)
```
You may need the Java Development Kit (JDK) in addition to the JRE in order to compile and run some specific Java-based software. To install the JDK, execute the following command, which will also install the JRE:
```
sudo apt install default-jdk
```
Verify that the JDK is installed by checking the version of javac, the Java compiler:
```
javac -version
```
You'll see the following output:
```
javac 10.0.1
```
Next, let's look at specifying which OpenJDK version we want to install.

### Installing Specific Versions of OpenJDK
While you can install the default OpenJDK package, you can also install different versions of OpenJDK.

#### OpenJDK 8
Java 8 is the current Long Term Support version and is still widely supported, though public maintenance ends in January 2019. 
To install OpenJDK 8, execute the following command:
```
sudo apt install openjdk-8-jdk
```
Verify that this is installed with
```
java -version
```
You'll see output like this:
```
openjdk version "1.8.0_162"
OpenJDK Runtime Environment (build 1.8.0_162-8u162-b12-1-b12)
OpenJDK 64-Bit Server VM (build 25.162-b12, mixed mode)
```
It is also possible to install only the JRE, which you can do by executing ```sudo apt install openjdk-8-jre```.

#### OpenJDK 10/11
Ubuntu's repositories contain a package that will install either Java 10 or 11. Prior to September 2018, this package will install OpenJDK 10. Once Java 11 is released, this package will install Java 11.

To install OpenJDK 10/11, execute the following command:
```
sudo apt install openjdk-11-jdk
```
To install the JRE only, use the following command:
```
sudo apt install openjdk-11-jre
```
### Managing Java
You can have multiple Java installations on one server. You can configure which version is the default for use on the command line by using the ```update-alternatives command```.
```
sudo update-alternatives --config java
```
This is what the output would look like if you've installed all versions of Java in this tutorial:
```
There are 3 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                            Priority   Status
------------------------------------------------------------
* 0            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1101      auto mode
  1            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1101      manual mode
  2            /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java   1081      manual mode
  3            /usr/lib/jvm/java-8-oracle/jre/bin/java          1081      manual mode
```
Choose the number associated with the Java version to use it as the default, or press ENTER to leave the current settings in place.

4.修改全局配置文件
```
sudo vim /etc/profile 
```
4.1 添加如下配置:
```
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export JRE_HOME=${JAVA_HOME}/jre
```
4.2 使修改的配置立刻生效
```
source /etc/profile
```
5.检查是否安装成功
```
java -version
```
## Maven
[官网链接](https://maven.apache.org/download.cgi)在此

2.移动到自己指定的位置
```
sudo mv apache-maven-3.6.1-bin.tar.gz /usr/local/
```
3.解压
```
sudo tar -zxvf apache-maven-3.6.1-bin.tar.gz  
sudo rm -rf apache-maven-3.6.1-bin.tar.gz 
```
解压之后的文件夹名字为：
```
apache-maven-3.6.1
```
4.修改全局配置文件
```
sudo vim /etc/profile 
```
4.1 添加如下配置:
```
export M2_HOME=/usr/local/apache-maven-3.6.1
export PATH=${M2_HOME}/bin:$PATH
```
4.2 使修改的配置立刻生效
```
source /etc/profile
```
5.检查是否安装成功
```
mvn -v
```
## Tomcat
到官网下载tomcat8.5.9，选择格式为tar.gz

因为我的默认下载目录就是```下载```，所以：
```
cd 下载
sudo tar -zxvf apache-tomcat-8.5.9.tar.gz
```
先在```/usr```下新建文件夹```tomcat```，然后将文件夹```apache-tomcat-8.5.9```移动到目录```/usr/tomcat```下：
```
sudo mv apache-tomcat-8.5.9 /usr/tomcat/
```
现在先修改一下tomcat文件夹的使用权限，否则可能在当前用户下不能进入bin目录：
```
cd /usr
sudo chmod 755 -R tomcat
```
然后进入目录```/usr/tomcat/apache-tomcat-8.5.9/bin```，编辑文件```startup.sh```，在最后一行之前加入如下信息：
```
#set java environment
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:%{JAVA_HOME}/lib:%{JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH

#tomcat
export TOMCAT_HOME=/usr/tomcat/apache-tomcat-8.5.9
```
其中```JAVA_HOME```和```TOMCAT_HOME```请对应你自己的jdk和tomcat的安装目录。编辑完后保存退出，然后运行```startup.sh```：
```
sudo ./startup.sh
```

如果要关闭tomcat，类似的，需要先在文件```shutdown.sh```对应位置添加信息：
```
#set java environment
export JAVA_HOME=/usr/java/jdk1.8.0_111
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:%{JAVA_HOME}/lib:%{JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH

#tomcat
export TOMCAT_HOME=/usr/tomcat/apache-tomcat-8.5.9
```
然后执行如下命令即可：
```
sudo ./shutdown.sh
```
出现如下信息则说明tomcat安装成功，并且已经启动。
### 插曲
运行```sudo ./shutdown.sh```的时候可能会出一点bug，意思就是找不到Java在哪，这个时候
```
sudo vim catalina.sh
```
在该文件里的最上方加入
```
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export JRE_HOME=${JAVA_HOME}/jre
```
即可
## Setting Environment Variables
以上设置路径什么的时候比较乱和零碎，以下是完整的配置内容：
```
sudo vim /etc/profile
```
加入以下内容，上面加入过的自动忽略，这是完整的。
```
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:%{JAVA_HOME}/lib:%{JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
export M2_HOME=/usr/local/apache-maven-3.6.1
export PATH=${M2_HOME}/bin:$PATH

#tomcat
export TOMCAT_HOME=/usr/tomcat/apache-tomcat-8.5.9
```
To activate it:
```
source /etc/profile
```
validation:
```
java -version
mvn -v
```
In browser, enter ```http://localhost:8080``` to see if you can see tomcat website.
## IDEA
这个其实没什么问题，去官网下载就好了，只不过也属于后端就放到一起了。没什么好说的。
## Reference
- [Java](https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-18-04)
- [Maven](https://blog.csdn.net/weixx3/article/details/80331538)
- [Tomcat](https://blog.csdn.net/xiangwanpeng/article/details/54561688)
- [IDEA]()
