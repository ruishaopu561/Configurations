# mySQL
## common order
```sudo mysql```直接可以进去
## update password
### find 'validate_password_policy'
```
mysql> select plugin_name, plugin_status from information_schema.plugins where plugin_name like 'validate%';

Empty set (0.00 sec)
```
```
mysql> install plugin validate_password soname 'validate_password.so';

Query OK, 0 rows affected (0.02 sec
```
```
mysql> select plugin_name, plugin_status from information_schema.plugins where plugin_name like 'validate%';

+-------------------+---------------+
| plugin_name | plugin_status |
+-------------------+---------------+
| validate_password | ACTIVE |
+-------------------+---------------+
1 row in set (0.00 sec)
```
### process
1.进入/etc/mysql/目录， 并用root权限打开debian.cnf文件  
2.使用这个文件中的用户名和密码进入mysql  
```mysql -u debian-sys-maint -p```  
3.选择mysql数据库  
```use mysql;```  
4.显示表中的列  
```show fields from user;```
```authentication_string```这列就是密码  
5.修改密码  
```update mysql.user set authentication_string=password('new password') where user='root'```  
6.退出  
7.重启  
```service mysql restart```
### about update password
mySQL的密码设置有了新的规则，所以在step5中设置新密码是往往会报错，不用担心。这很正常，输入一下指令查看mysql全局参数配置
```select @@validate_password_policy```  
```show variables like 'validate_password%';```查看具体的规则，里面各个参数的具体含义很明显，详见Reference.
## Reference
- [unknown variable 'validate_password_policy=0'](https://github.com/MoonlitNight/shell/issues/1)
- [update password](https://blog.csdn.net/xuxile/article/details/78053496)
- [validate of password](https://blog.csdn.net/kuluzs/article/details/51924374)
