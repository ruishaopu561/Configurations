# Django on Ubuntu18.04
## Install Python and PIP
Most of the latest operating systems come with default Python 3 installed. But if your system doesn’t have Python installed, Execute the below commands to install it. Also, install pip on your system.
```
sudo apt-get install python3 python3-pip
```
The installed Python version is:
```
python3 -V
```
```
Python 3.5.3
```
and pip version is:
```
pip3 -V
```
```
pip 9.0.1 from /usr/lib/python3/dist-packages (python 3.5)
```
## Install Django on Ubuntu
Django source code is available as Github repository. You can also use pip to install Django on Ubuntu 18.04 systems. In this tutorial, I use pip3 for the Django installation on Ubuntu. Run the below command from Linux terminal:
```
pip3 install Django
```
You will get a django-admin command for creating new projects. Check the current installed verson:
```
django-admin --version
```
```
2.1.2
```
## Create A Django Application
The django-admin command provides you option to create a new Django application via command line. First, navigate to the directory you need to create a new application.

Then use django-admin startproject command followed by the application name to create new Django application
```
cd /var/www
django-admin startproject django_app
```
After that migrate the pending changes.
```
cd django_app
python3 manage.py migrate
```
![usual result](https://github.com/ruishaopu561/Configurations/blob/master/Django/django-migrate-changes.png)
## Create Super User
Also, create a superuser account for the administration of the Django application. Run the following command from your Django application directory.
```
python3 manage.py createsuperuser
```
## Run Django Application
Your Django application is ready to use. By default, Django doesn’t allow external hosts to access the web interface. To allow external hosts, edit settings.py file and add IP under ALLOWED_HOSTS.
```
vi django_app/settings.py
```
Add IP:
```
ALLOWED_HOSTS = ['192.168.1.239','localhost']
```
Here 192.168.1.239 is the IP address of the system where Django is installed.

Finally, run the Django application server with below command. Here 0.0.0.0:8000 defined that Django will listen on all interfaces on port 8000. You can change this port with any of your choices.
```
python3 manage.py runserver 0.0.0.0:8000
```
Django application server is running now. Open your favorite web browser and access to Django system ip on port 8000. This will show you the default Django web page.
```
http://192.168.1.239:8000
or
http://localhost:8000
```
![web interface](https://github.com/ruishaopu561/Configurations/blob/master/Django/django-web-interface.png)

Django also provides an administrative web interface. You can access this at /admin subdirectory URL of your Django application. Use superuser login credentials created in the previous step.
```
http://192.168.1.239:8000/admin
or
http://localhost:8000/admin
```
![admin login](https://github.com/ruishaopu561/Configurations/blob/master/Django/django-admin-login.png)

The Django admin dashboard looks like below. Here you can add more users and groups for your application.

![admin dashboard](https://github.com/ruishaopu561/Configurations/blob/master/Django/django-admin-dashboard.png)

## Reference
- [Django基础搭建](https://tecadmin.net/install-django-on-ubuntu/)
