# travis CI
利用 Travis CI 自动测试、部署 Java Web 项目到 Tomcat

![travis](https://github.com/ruishaopu561/Configurations/TravisCI/travis.png)
上一个项目的部署测试流程是：本地写完，本地测试，打包为 war 上传到服务器，服务器部署到 Tomcat 指定目录下，重启 Tomcat。这一套流程下来少说十分钟，而且如果刚上传完发现有 bug 的话，还要本地改完重新再来一遍。重复这样的过程让人心神俱疲，好在现在已经有成熟的解决方案如 Travis CI、Jenkins等，今天我们就尝试着在 Java Web 项目中运用 Travis CI 来自动测试、部署、重启。

Travis CI 是软件开发领域一个在线的、分布式的持续集成服务，它与 GitHub 的协作相当紧密，并且对开源项目免费。

## 注册配置 Travis
到 Travis 的[官网](https://travis-ci.org)注册登录，其实是用 GitHub 做第三方授权登录，十分简单。

![Travis Register](https://github.com/ruishaopu561/Configurations/TravisCI/travis-register.png)

注册成功后进入个人 Profile 页，选择一个需要集成 Travis CI 的项目，开启 Build 即可。

## Test Demo
### 创建项目
本文我们以一个简单的 Spring Boot 项目为例：

[项目地址](https://github.com/ruishaopu561/Backend/task3/task3)

### 添加 .travis.yml
Travis 需要根据项目中 ```.travis.yml``` 文件中的配置信息来执行相应的动作。
```
language: java
jdk:
 - openjdk8
install: cd Hello && mvn install -DskipTests=true -Dmaven.javadoc.skip=true
script: mvn test
```
注意 install 的一行，根据目录树，我们的项目是在 Hello 目录下的，所以 install 的时候需要先切换到 Hello 目录中，否则 Maven 会找不到 ```pom.xml``` 进而构建失败。

### 触发构建
提交并 push 到 GitHub 后，Travis就会自动构建这个 Maven 工程，可以在 Travis 上看到构建结果和过程中的详细输出：

![Travis Build](https://github.com/ruishaopu561/Configurations/TravisCI/travis-build.png)

## .travis.yml 语法简介
[官网](https://docs.travis-ci.com/)

## Java
因为现在学的但是java，所以这里只写java相关的语法，以后学了新的再补充。
### Overview
The Travis CI environment contains various versions of OpenJDK, Oracle JDK, Gradle, Maven and Ant.

To use the Java environment, add the following to your ```.travis.yml```:

```yml
language: java
```

### Projects Using Maven
#### Maven Dependency Management
Before running the build, Travis CI installs dependencies:
```zsh
mvn install -DskipTests=true -Dmaven.javadoc.skip=true -B -V
```
or if your project uses the ```mvnw``` wrapper script:
```zsh
./mvnw install -DskipTests=true -Dmaven.javadoc.skip=true -B -V
```

```
Note that the Travis CI build lifecycle and the Maven build lifecycle use similar terminology for different build phases. For example, install in a Travis CI build comes much earlier than install in the Maven build lifecycle. More details can be found about the Travis Build Lifecycle and the Maven Build Lifecycle.
```

#### Maven Default Script Command #
If your project has ```pom.xml``` file in the repository root but no ```build.gradle```, Travis CI builds your project with Maven 3:
```zsh
mvn test -B
```
If your project also includes the mvnw wrapper script in the repository root, Travis CI uses that instead:
```zsh
./mvnw test -B
```
```
The default command does not generate JavaDoc (-Dmaven.javadoc.skip=true).
```
To use a different ```script``` command, customize the [build step](https://docs.travis-ci.com/user/job-lifecycle/#customizing-the-build-phase).

### Projects Using Gradle
#### Gradle Dependency Management
Before running the build, Travis CI installs dependencies:
```zsh
gradle assemble
```
or
```zsh
./gradlew assemble
```
To use a different ```install``` command, customize the [installation step](https://docs.travis-ci.com/user/job-lifecycle/#customizing-the-installation-phase).

#### Gradle Default Script Command
If your project contains a ```build.gradle``` file in the repository root, Travis CI builds your project with Gradle:
```zsh
gradle check
```
If your project also includes the ```gradlew``` wrapper script in the repository root, Travis CI uses that wrapper instead:
```zsh
./gradlew check
```
To use a different ```script``` command, customize the [build step](https://docs.travis-ci.com/user/job-lifecycle/#customizing-the-build-phase).

#### Caching
A peculiarity of dependency caching in Gradle means that to avoid uploading the cache after every build you need to add the following lines to your ```.travis.yml```:

```yml
before_cache:
  - rm -f  $HOME/.gradle/caches/modules-2/modules-2.lock
  - rm -fr $HOME/.gradle/caches/*/plugin-resolution/
cache:
  directories:
    - $HOME/.gradle/caches/
    - $HOME/.gradle/wrapper/
```
```
Note that if you use Gradle with sudo (i.e. sudo ./gradlew assemble), the caching configuration above will have no effect, since the depencencies will be in /root/.gradle which the travis user account does not have write access to.
```

### Projects Using Ant
#### Ant Dependency Management
Because there is no single standard way of installing project dependencies with Ant, you need to specify the exact command to run using ```install:``` key in your ```.travis.yml```, for example:

```yml
language: java
install: ant deps
```

#### Ant Default Script Command
If Travis CI does not detect Maven or Gradle files it runs Ant:
```zsh
ant test
``` 
To use a different ```script``` command, customize the [build step](https://docs.travis-ci.com/user/job-lifecycle/#customizing-the-build-phase).

### Testing Against Multiple JDKs
To test against multiple JDKs, use the ```jdk:``` key in ```.travis.yml```. For example, to test against Oracle JDKs 8 and 9, as well as OpenJDK 8:
```yml
jdk:
  - oraclejdk8
  - oraclejdk9
  - openjdk8
```
```
Note that testing against multiple Java versions is not supported on macOS. See the macOS Build Environment for more details.
```
The list of available JVMs for different dists are at

- [JDKs installed for Trusty](https://docs.travis-ci.com/user/reference/trusty/#jvm-clojure-groovy-java-scala-images)
- [JDKs installed for Precise](https://docs.travis-ci.com/user/reference/precise/#jvm-clojure-groovy-java-scala-vm-images)
#### Switching JDKs (Java 8 and below) Within One Job
If your build needs to switch JDKs (Java 8 and below) during a job, you can do so with ```jdk_switcher use …```.
```yml
script:
  - jdk_switcher use oraclejdk8
  - # do stuff with Java 8
  - jdk_switcher use openjdk8
  - # do stuff with open Java 8
```
Use of ```jdk_switcher``` also updates ```$JAVA_HOME``` appropriately.

#### Updating Oracle JDK 8 to a recent release
Your repository may require a newer release of Oracle JDK than the pre-installed version.

The following example will use the latest Oracle JDK 8:
```yml
addons:
  apt:
    packages:
      - oracle-java8-installer
```
### Using Java 10 and later
```
Take note that oraclejdk10 is EOL since October 2018 and as such it’s not supported anymore on Travis CI. See https://www.oracle.com/technetwork/java/javase/eol-135779.html.
```
OracleJDK 11 and later are supported on Linux, and OpenJDK 10 and later are supported on Linux and macOS using ```install-jdk.sh```.
```yml
jdk:
  - oraclejdk8
  - oraclejdk11
  - openjdk10
  - openjdk11
```
#### Switching JDKs (to Java 10 and up) Within One Job
If your build needs to switch JDKs (Java 8 and up) during a job, you can do so with ```install-jdk.sh```.

```yml
jdk: openjdk10
script:
  - jdk_switcher use openjdk10
  - # do stuff with OpenJDK 10
  - export JAVA_HOME=$HOME/openjdk11
  - $TRAVIS_BUILD_DIR/install-jdk.sh --install openjdk11 --target $JAVA_HOME
  - # do stuff with open OpenJDK 11
```

## Python
### Specifying Python versions
Specify python versions using the ```python``` key. As we update the Python build images, aliases like ```3.6``` will point to different exact versions or patch levels.
```yml
language: python
python:
  - "2.6"
  - "2.7"
  - "3.3"
  - "3.4"
  - "3.5"
  - "3.5-dev"  # 3.5 development branch
  - "3.6"
  - "3.6-dev"  # 3.6 development branch
# command to install dependencies
install:
  - pip install -r requirements.txt
# command to run tests
script:
  - pytest
```
#### Python 3.7 and higher
You’ll need to add ```dist: xenial``` to your ```.travis.yml``` file to use Python 3.7 and higher.

For example:
```yml
dist: xenial   # required for Python >= 3.7
language: python
python:
  - "3.7"
  - "3.7-dev"  # 3.7 development branch
  - "3.8-dev"  # 3.8 development branch
  - "nightly"  # nightly build
```
#### Travis CI Uses Isolated virtualenvs
The CI Environment uses separate virtualenv instances for each Python version. This means that as soon as you specify ```language: python``` in ```.travis.yml``` your tests will run inside a virtualenv (without you having to explicitly create it). System Python is not used and should not be relied on. If you need to install Python packages, do it via pip and not apt.

If you decide to use apt anyway, note that for compatibility reasons, you’ll only be able to use the default Python versions that are available in Ubuntu (e.g. for Trusty, this means 2.7.6 and 3.4.3). To access the packages inside the virtualenv, you will need to specify that it should be created with the ```--system-site-packages``` option. To do this, include the following in your ```.travis.yml```:
```yml
language: python
python:
  - "2.7"
  - "3.4"
virtualenv:
  system_site_packages: true
```
#### PyPy Support
Travis CI supports PyPy and PyPy3.

To test your project against PyPy, add “pypy3.5” to the list of Pythons in your ```.travis.yml```:
```yml
language: python
python:
  - "2.7"
  - "3.5"
  - "3.6"
  # PyPy versions
  - "pypy3.5"
# command to install dependencies
install:
  - pip install -r requirements.txt
  - pip install .
# command to run tests
script: pytest
```
#### Nightly build support
Travis CI supports a special version name ```nightly```, which points to a recent development version of [CPython](https://github.com/python/cpython) build.

#### Development releases support
From Python 3.5 and later, Python In Development versions are available.

You can specify these in your builds with ```3.5-dev```, ```3.6-dev```, ```3.7-dev``` or ```3.8-dev```.
```
Recent Python branches require OpenSSL 1.0.2+. As this library is not available for Trusty, 3.7, 3.7-dev, 3.8-dev, and nightly do not work (or use outdated archive).
``
### Default Build Script
Python projects need to provide the ```script``` key in their ```.travis.yml``` to specify what command to run tests with.

For example, if your project uses pytest:
```yml
# command to run tests
script: pytest
```
if it uses ```make test``` instead:
```yml
script: make test
```
If you do not provide a ```script``` key in a Python project, Travis CI prints a message (“Please override the script: key in your .travis.yml to run tests.”) and fails the build.

### Using Tox as the Build Script
Due to the way Travis is designed, interaction with tox is not straightforward. As described above, Travis already runs tests inside an isolated virtualenv whenever ```language: python``` is specified, so please bear that in mind whenever creating more environments with tox. If you would prefer to run tox outside the Travis-created virtualenv, it might be a better idea to use ```language: generic``` instead of ```language: python```.

If you’re using tox to test your code against multiple versions of python, you have two options:

use ```language: generic``` and manually install the python versions you’re interested in before running tox (without the manual installation, tox will only have access to the default Ubuntu python versions - 2.7.6 and 3.4.3 for Trusty)
use ```language: python``` and a build matrix that uses a different version of python for each branch (you can specify the python version by using the ```python``` key). This will ensure the versions you’re interested in are installed and parallelizes your workload.
A good example of a ```travis.yml``` that runs tox using a Travis build matrix is [twisted/klein](https://github.com/twisted/klein/blob/master/.travis.yml).

### Running Python tests on multiple Operating Systems
Sometimes it is necessary to ensure that software works the same across multiple Operating Systems. This following ```.travis.yml``` file will execute parallel test runs on Linux, macOS, and Windows.
```yml
language: python            # this works for Linux but is an error on macOS or Windows
matrix:
  include:
    - name: "Python 3.7.1 on Xenial Linux"
      python: 3.7           # this works for Linux but is ignored on macOS or Windows
      dist: xenial          # required for Python >= 3.7
    - name: "Python 3.7.2 on macOS"
      os: osx
      osx_image: xcode10.2  # Python 3.7.2 running on macOS 10.14.3
      language: shell       # 'language: python' is an error on Travis CI macOS
    - name: "Python 3.7.3 on Windows"
      os: windows           # Windows 10.0.17134 N/A Build 17134
      language: shell       # 'language: python' is an error on Travis CI Windows
      before_install: choco install python
      env: PATH=/c/Python37:/c/Python37/Scripts:$PATH
install: pip3 install --upgrade pip  # all three OSes agree about 'pip3'
# 'python' points to Python 2.7 on macOS but points to Python 3.7 on Linux and Windows
# 'python3' is a 'command not found' error on Windows but 'py' works on Windows only
script: python3 my_app.py || python my_app.py
```
### Dependency Management
#### pip
By default Travis CI uses ```pip``` to manage Python dependencies. If you have a ```requirements.txt``` file, Travis CI runs ```pip install -r requirements.txt``` during the ```install``` phase of the build.

You can manually override this default ```install``` phase, for example:
```yml
install: pip install --user -r requirements.txt
```
Please note that the ```--user``` option is mandatory if you are not using ```language: python```, since no virtualenv will be created in that case.

#### Custom Dependency Management
To override the default ```pip``` dependency management, alter the ```before_install``` step as described in [general build configuration](https://docs.travis-ci.com/user/job-lifecycle/#customizing-the-installation-phase) guide.

#### Testing Against Multiple Versions of Dependencies (e.g. Django or Flask)
If you need to test against multiple versions of, say, Django, you can instruct Travis CI to do multiple runs with different sets or values of environment variables.

Use env key in your .travis.yml file, for example:
```yml
env:
  - DJANGO_VERSION=1.10.8
  - DJANGO_VERSION=1.11.5
```
and then use ENV variable values in your dependencies installation scripts, test cases or test script parameter values. Here we use ENV variable value to instruct pip to install an exact version:
```yml
install:
  - pip install -q Django==$DJANGO_VERSION
  - python setup.py -q install
```
The same technique is often used to test projects against multiple databases and so on.

For a real world example, see [getsentry/sentry](https://github.com/getsentry/sentry/blob/master/.travis.yml) and [jpvanhal/flask-split](https://github.com/jpvanhal/flask-split/blob/master/.travis.yml).

## Reference
- [travis CI](https://xlui.me/t/travis-tomcat/)
- [travis grammar](https://docs.travis-ci.com/)