## CentOS��Jenkins��ʹ��
----------------
### ׼������
Jenkins�İ�װҪ��֤ϵͳ���Ѿ���װ����jdk�������jdk1.5����
### ��װjava
`sudo yum install java`  ������������⣬��ο��������⼰���������
### ��װjenkins
`sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo`
`sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key`
`sudo yum install jenkins`
### ��װjenkins�ȶ��汾
`sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo`  
`sudo rpm --import http://pkg.jenkins-ci.org/redhat-stable/jenkins-ci.org.key`
`sudo yum install jenkins`
### ����jenkins
��������jenkins�������俪������  
`sudo service jenkins restart`  
`sudo chkconfig jenkins on`  
* ����һ  
�л���jenkins.war��ŵ�Ŀ¼�������������
`java -jar jenkins.war`
��������У��Ƽ���������룺 **IP��ַ:8080��Ĭ��Ϊ8080�˿ڣ������Ҫʹ�ñ�Ķ˿ڣ�����ὲ����**�Ϳ��Դ�jenkins��
* ������  
ʹ��tomcat�򿪣���ѹtomcat��ĳ��Ŀ¼�£���/usr/local������tomcat/binĿ¼�£�����tomcat����jenkin.war�ļ�����tomcat�µ�webappsĿ¼�£�����jenkinsʱ�����Զ���webappsĿ¼�½���jenkinsĿ¼�������ڵ�ַ������Ҫ����ĵ�ַ�ͷ���һ�е㲻ͬ��  

*�����������������һЩ���⣬�޷������������鿴�������⼰�������*  

----------------------
### ��װ���������������⼰�������
#### ����1  ����������jenkins�������´�����ʾ��  
`Starting jenkins (via systemctl):  Job for jenkins.service failed. 
See 'systemctl status jenkins.service' and 'journalctl -xn' for details.
                                                           [FAILED]`   
#### �������  
���ִ����⣬���п�������Ϊjavaδ����װ�����߰�װ��java�汾����ȷ��ʹ������`sudo java -version`�鿴java�汾������������������£�  
java -version  
java version "1.5.0"  
gij (GNU libgcj) version 4.4.6 20110731 (Red Hat 4.4.6-3)  
˵��������ʹ��Ĭ�ϵ�GCJ�����ܺ�jenkins���ݣ���ô��ʹ�������������°�װ��  
`sudo yum remove java`  
`sudo yum install java-1.6.0-openjdk`  

#### ����2 ����ʱ����update.xml������  
���ָ���������Ϊjenkins�Դ���update.xml�ļ���ĸ���·�����ԣ�����update.xml�ļ����·�������Ƶ��������ʱ���Զ���ת���µ�·�������µ�·�����Ƶ�update.xml�У������������ɡ�

#### ����3 ����ʱ���ɹ�--����ǽ������  
���ʹ�����������ɹ������п�������Ϊ����ǽ�����⣬ʹ����������ŷ���ǽ8080�˿ڣ�ʹ����Խ��з��ʡ�  
`firewall-cmd --zone=public --add-port=8080/tcp --permanent`  
`firewall-cmd --zone=public --add-service=http --permanent`  
`firewall-cmd --reload`  
`firewall-cmd --list-all`    

#### jenkins�˿�ʹ��   
���������jenkinsʱ��ʹ��Ĭ�ϵ�8080�˿ڣ��������¹��ڶ˿ڲ��������  
 --httpPort=8888        ʹ��8888�˿���Ϊjenkins�ķ��ʶ˿�   
 --prefix=/jenkins      ���ӷ���·����ǰ׺����ԭ������ʱΪhttp://127.0.0.1:8080���������Ϊ
                         http://127.0.0.1:8080/jenkins  
 --httpListenAddress=127.0.0.1     �󶨼����ĵ�ַ




 

