jetty的运行需要jdk的支持，所以首先要安装jdk，因为项目需要，安装的是jdk版本是8及以上，其实ubuntu上配置JDK的教程已经很多，我简单列一下过程：
### 1.安装JDK 
(1)查看主机是32位还是64位

(2)下载jdk8包
首先创建java的文件位置并赋予权限：  
`sudo mkdir -p /usr/local/java`  
`sudo chmod 755 /usr/local/java`  
进入该目录：  
`cd /usr/local/java`  

32位：`sudo wget http://download.oracle.com/otn-pub/java/jdk/8u45-b14/jdk-8u45-linuxi586.tar.gz`  

64位:`sudo wget http://download.oracle.com/otn-pub/java/jdk/8u45-b14/jdk-8u45-linux-x64.tar.gz`  

(3)解压到当前目录  
`sudo tar xvzf jdk-8u45-linux-x64.tar.gz`  

(4)设置JDK环境变量  

打开配置文件：  
`sudo vim /etc/profile`  
  
在该文件中添加如下内容：
``` 
# set jdk env  
export JAVA_HOME=/usr/local/java/jdk-8u45-linux-x64  
export JAVA_JRE=/usr/local/java/jdk-8u45-linux-x64/jre   
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/
lib/tools.jar:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH 
export PATH=$JAVA_HOME/bin:$PATH

```  
更新配置文件：  
`source /etc/profile`  

验证JDK是否安装成功：  
`java -version`  
如果显示出了java的版本号，则说明安装正确  

--------------

### 2.安装并运行jetty  
(1)在[这里](http://download.eclipse.org/jetty/stable-9/dist/)下载jetty9，然后使用如下命令来解压缩：    
`tar xvzf jetty-distribution-9.2.5.v20141112.tar.gz`  
  
(2)该操作会产生文件jetty-distribution-9.2.5.v20141112，然后将该文件归档，使用命令：    
`mv jetty-distribution-9.2.5.v20141112 /opt/jetty`  
  
(3)创建jetty用户，然后将其设置成/opt/jetty目录的属主  
`sudo useradd jetty -U -s /bin/false`  
`sudo chown -R jetty:jetty /opt/jetty`  

(4)使用以下命令拷贝jetty运行脚本到启动目录，以便让其作为一个服务来运行  
`sudo cp /opt/jetty/bin/jetty.sh /etc/init.d/jetty`  

(5)创建jetty的设置文件  
`sudo vim /etc/default/jetty`  
然后在文件中添加如下内容：
```  
JAVA_HOME=/usr/local/java/jdk-8u45-linux-x64
JETTY_HOME=/opt/jetty
NO_START=0
JETTY_ARGS=jetty.port=4006
JETTY_HOST=0.0.0.0
JETTY_USER=jetty
```  
按下 `ECS` 键，然后 `:wq` 进行保存并退出  

(6)然后使用如下命令来启动jetty服务：  
`sudo service jetty start`  
使用如下命令可以验证并检查jetty的配置：  
`sudo service jetty check`  
使用如下命令可以重启服务器并测试jetty是否自启动：  
`sudo update-rc.d jetty defaults`  

(7)将自己已经写好的文件放到jetty目录的/webapps目录下(本例中即/opt/jetty/webapps目录下面)，然后在浏览器中即可访问：  
`http://ip-host:4006`
