## CentOS上Jenkins的使用
----------------
### 准备工作
Jenkins的安装要保证系统中已经安装好了jdk，最好是jdk1.5以上
### 安装java
`sudo yum install java`  （如果出现问题，请参考后面问题及解决方法）
### 安装jenkins
`sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo`
`sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key`
`sudo yum install jenkins`
### 安装jenkins稳定版本
`sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo`  
`sudo rpm --import http://pkg.jenkins-ci.org/redhat-stable/jenkins-ci.org.key`
`sudo yum install jenkins`
### 启动jenkins
首先重启jenkins并设置其开机启动  
`sudo service jenkins restart`  
`sudo chkconfig jenkins on`  
* 方法一  
切换到jenkins.war存放的目录，输入如下命令：
`java -jar jenkins.war`
在浏览器中（推荐火狐）输入： **IP地址:8080（默认为8080端口，如果需要使用别的端口，后面会讲到）**就可以打开jenkins。
* 方法二  
使用tomcat打开，解压tomcat到某个目录下，如/usr/local，进入tomcat/bin目录下，启动tomcat，将jenkin.war文件放入tomcat下的webapps目录下，启动jenkins时，会自动在webapps目录下建立jenkins目录，所以在地址栏上需要输入的地址和方法一有点不同。  

*如果启动过程中遇到一些问题，无法正常启动，查看后面问题及解决方法*  

----------------------
### 安装过程中遇到的问题及解决方法
#### 错误1  启动或重启jenkins出现如下错误提示：  
`Starting jenkins (via systemctl):  Job for jenkins.service failed. 
See 'systemctl status jenkins.service' and 'journalctl -xn' for details.
                                                           [FAILED]`   
#### 解决方法  
出现此问题，很有可能是因为java未被安装，或者安装的java版本不正确，使用命令`sudo java -version`查看java版本，如果出现类似于如下：  
java -version  
java version "1.5.0"  
gij (GNU libgcj) version 4.4.6 20110731 (Red Hat 4.4.6-3)  
说明你正在使用默认的GCJ，不能和jenkins兼容，那么需使用如下命令重新安装：  
`sudo yum remove java`  
`sudo yum install java-1.6.0-openjdk`  

#### 错误2 启动时出现update.xml的问题  
出现该问题是因为jenkins自带的update.xml文件里的更新路径不对，复制update.xml文件里的路径，复制到浏览器打开时会自动跳转到新的路径，将新的路径复制到update.xml中，重新启动即可。

#### 错误3 启动时不成功--防火墙的问题  
访问过程中如果不成功，很有可能是因为防火墙的问题，使用如下命令开放防火墙8080端口，使其可以进行访问。  
`firewall-cmd --zone=public --add-port=8080/tcp --permanent`  
`firewall-cmd --zone=public --add-service=http --permanent`  
`firewall-cmd --reload`  
`firewall-cmd --list-all`    

#### jenkins端口使用   
如果在启动jenkins时不使用默认的8080端口，则有如下关于端口操作的命令：  
 --httpPort=8888        使用8888端口作为jenkins的访问端口   
 --prefix=/jenkins      增加访问路径的前缀，如原来访问时为http://127.0.0.1:8080，现在则变为
                         http://127.0.0.1:8080/jenkins  
 --httpListenAddress=127.0.0.1     绑定监听的地址




 

