# Tomcat

https://tomcat.apache.org/

## Installation

[Apache Tomcat Versions](http://tomcat.apache.org/whichversion.html)

[Tomcat 8 Software Downloads](http://tomcat.apache.org/download-80.cgi)

[Apache Taglibs Downloads](http://tomcat.apache.org/download-taglibs.cgi)

**[RUNNING.txt](http://tomcat.apache.org/tomcat-8.5-doc/RUNNING.txt) （安装和配置的详细说明）**

### Linux

#### Installing JDK
Refers to `Java Practice`

#### Installing Tomcat
```
# Creating the installation directory
$ export INSTALL_ROOT=/usr/local/tomcat; mkdir $INSTALL_ROOT; cd $INSTALL_ROOT

# Downloading archive file and unpack it
$ wget http://apache.website-solution.net/tomcat/tomcat-8/v8.5.20/bin/apache-tomcat-8.5.20.tar.gz; tar zxvf apache-tomcat-8.5.20.tar.gz

# Creating Symlink
$ ln -s apache-tomcat-8.5.20 default

# Verifying
$ $INSTALL_ROOT/default/bin/catalina.sh start
```

Start Up Tomcat
```bash
$CATALINA_HOME/bin/startup.sh
or
$CATALINA_HOME/bin/catalina.sh start
```

Shut Down Tomcat
```bash
$CATALINA_HOME/bin/shutdown.sh
or
$CATALINA_HOME/bin/catalina.sh stop
```

### Mac OS X



### Windows

**Set `CATALINA_HOME` (required) and `CATALINA_BASE` (optional)**

The `CATALINA_HOME` environment variable should be set to the location of the
root directory of the "binary" distribution of Tomcat.
```
CATALINA_HOME=C:\Program Files\apache-tomcat-8.5.27
```

> 新建环境变量：快捷键 `Win + Pause` -> 高级系统设置 -> 环境变量 -> 新建

**Set `JRE_HOME` or `JAVA_HOME` (required)**

These variables are used to specify location of a Java Runtime
Environment or of a Java Development Kit that is used to start Tomcat.
```
JAVA_HOME=C:\Program Files\Java\jdk1.8.0_161
```

**Start Up Tomcat**
```dos
%CATALINA_HOME%\bin\startup.bat
or
%CATALINA_HOME%\bin\catalina.bat start
```

**Shut Down Tomcat**
```dos
%CATALINA_HOME%\bin\shutdown.bat
or
%CATALINA_HOME%\bin\catalina.bat stop
```

**Verifying**

Open http://localhost:8080/

## Docker
https://hub.docker.com/_/tomcat/

## [Configuration Reference](http://tomcat.apache.org/tomcat-8.5-doc/config/index.html)

## Documentation
Documentation Index
http://tomcat.apache.org/tomcat-8.5-doc/index.html

Introduction
http://tomcat.apache.org/tomcat-8.5-doc/introduction.html

通过容器的8080端口访问
docker run -it --rm tomcat:8.0

通过宿主机的8888端口访问
docker run -it --rm -p 8888:8080 tomcat:8.0
docker run -it --rm -p 8888:8080 -v /data/tomcat/webapps:/usr/local/tomcat/webapps tomcat:7.0
