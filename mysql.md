### Ubuntu14.04下搭建可远程访问的Mysql服务器  
1.更新源  
`source /etc/apt/source.list`  
`sudo apt-get update`  
2.在Ubuntu14.04下安装mysql：  
`sudo apt-get install mysql-server`  
3.在目录/etc/mysql下打开my.cnf，用vim编辑，找到  
`bind-address   =127.0.0.1`  
改为：  
`bind-address   =0.0.0.0`或者直接将上句注释掉  
4.使用root账户登录到Mysql数据库：  
`mysql -u root -p`  
使用命令：  
`use mysql;`  
`select host, user, password from user`可以查看到当前可以连接到该服务器的用户  
在mysql>输入：  
`grant all on *.* to root@'%' identified by '123'`  
注：  
* root是用户名
* passwd是连接密码  
`flush privileges;`  # 刷新权限表  
5.最后在远端可以进行远程登录：  
* 用户名：root
* 密码：123
* 端口：3306  
完成！
