### Ubuntu14.04�´��Զ�̷��ʵ�Mysql������  
1.����Դ  
`source /etc/apt/source.list`  
`sudo apt-get update`  
2.��Ubuntu14.04�°�װmysql��  
`sudo apt-get install mysql-server`  
3.��Ŀ¼/etc/mysql�´�my.cnf����vim�༭���ҵ�  
`bind-address   =127.0.0.1`  
��Ϊ��  
`bind-address   =0.0.0.0`����ֱ�ӽ��Ͼ�ע�͵�  
4.ʹ��root�˻���¼��Mysql���ݿ⣺  
`mysql -u root -p`  
ʹ�����  
`use mysql;`  
`select host, user, password from user`���Բ鿴����ǰ�������ӵ��÷��������û�  
��mysql>���룺  
`grant all on *.* to root@'%' identified by '123'`  
ע��  
* root���û���
* passwd����������  
`flush privileges;`  # ˢ��Ȩ�ޱ�  
5.�����Զ�˿��Խ���Զ�̵�¼��  
* �û�����root
* ���룺123
* �˿ڣ�3306  
��ɣ�
