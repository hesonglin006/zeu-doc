jetty��������Ҫjdk��֧�֣���������Ҫ��װjdk����Ϊ��Ŀ��Ҫ����װ����jdk�汾��8�����ϣ���ʵubuntu������JDK�Ľ̳��Ѿ��ܶ࣬�Ҽ���һ�¹��̣�
### 1.��װJDK 
(1)�鿴������32λ����64λ

(2)����jdk8��
���ȴ���java���ļ�λ�ò�����Ȩ�ޣ�  
`sudo mkdir -p /usr/local/java`  
`sudo chmod 755 /usr/local/java`  
�����Ŀ¼��  
`cd /usr/local/java`  

32λ��`sudo wget http://download.oracle.com/otn-pub/java/jdk/8u45-b14/jdk-8u45-linuxi586.tar.gz`  

64λ:`sudo wget http://download.oracle.com/otn-pub/java/jdk/8u45-b14/jdk-8u45-linux-x64.tar.gz`  

(3)��ѹ����ǰĿ¼  
`sudo tar xvzf jdk-8u45-linux-x64.tar.gz`  

(4)����JDK��������  

�������ļ���  
`sudo vim /etc/profile`  
  
�ڸ��ļ�������������ݣ�
``` 
# set jdk env  
export JAVA_HOME=/usr/local/java/jdk-8u45-linux-x64  
export JAVA_JRE=/usr/local/java/jdk-8u45-linux-x64/jre   
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/
lib/tools.jar:$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH 
export PATH=$JAVA_HOME/bin:$PATH

```  
���������ļ���  
`source /etc/profile`  

��֤JDK�Ƿ�װ�ɹ���  
`java -version`  
�����ʾ����java�İ汾�ţ���˵����װ��ȷ  

--------------

### 2.��װ������jetty  
(1)��[����](http://download.eclipse.org/jetty/stable-9/dist/)����jetty9��Ȼ��ʹ��������������ѹ����    
`tar xvzf jetty-distribution-9.2.5.v20141112.tar.gz`  
  
(2)�ò���������ļ�jetty-distribution-9.2.5.v20141112��Ȼ�󽫸��ļ��鵵��ʹ�����    
`mv jetty-distribution-9.2.5.v20141112 /opt/jetty`  
  
(3)����jetty�û���Ȼ�������ó�/opt/jettyĿ¼������  
`sudo useradd jetty -U -s /bin/false`  
`sudo chown -R jetty:jetty /opt/jetty`  

(4)ʹ�����������jetty���нű�������Ŀ¼���Ա�������Ϊһ������������  
`sudo cp /opt/jetty/bin/jetty.sh /etc/init.d/jetty`  

(5)����jetty�������ļ�  
`sudo vim /etc/default/jetty`  
Ȼ�����ļ�������������ݣ�
```  
JAVA_HOME=/usr/local/java/jdk-8u45-linux-x64
JETTY_HOME=/opt/jetty
NO_START=0
JETTY_ARGS=jetty.port=4006
JETTY_HOST=0.0.0.0
JETTY_USER=jetty
```  
���� `ECS` ����Ȼ�� `:wq` ���б��沢�˳�  

(6)Ȼ��ʹ����������������jetty����  
`sudo service jetty start`  
ʹ���������������֤�����jetty�����ã�  
`sudo service jetty check`  
ʹ�����������������������������jetty�Ƿ���������  
`sudo update-rc.d jetty defaults`  

(7)���Լ��Ѿ�д�õ��ļ��ŵ�jettyĿ¼��/webappsĿ¼��(�����м�/opt/jetty/webappsĿ¼����)��Ȼ����������м��ɷ��ʣ�  
`http://ip-host:4006`
