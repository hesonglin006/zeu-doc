### һ��Compose��װ   
�ڰ�װcompose֮ǰ��Ҫȷ���Ѿ���װ��docker1.3�����ϰ汾  
��Linux64λϵͳ�ϰ�װcompose��  
```
curl -L https://github.com/docker/compose/releases/download/1.1.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose 
chmod +x /usr/local/bin/docker-compose  
``` 
* ע����Ȼ����ѡ��װ`command completion`��������  
*     `uname -s`��`uname -m`�е����������Ǽ�����ESC������Ǹ�����   
��ʱ��compose�Ѿ���װ�ɹ���ʹ������`docker-compose --version`���Բ鿴 
 
�������OS Xϵͳ�ϣ�����Ҫִ�����²��裨δ�ײ⣩��
 
---------------------------------------------- 
### ����Compose���ȫ
#### ��װ���ȫ  
ȷ��bash completion�Ѿ���װ�������ǰʹ�÷���С��װ��Linux��bash completion�Ѿ�OK�ˣ��������MAC�ϣ�����ʹ��`brew install bash-completion`����װ  
��completion�ű�����/etc/bash_completion.d/����MAC����/usr/local/etc/bash_completion.d/��  
```
curl -L https://raw.githubusercontent.com/docker/compose/1.1.0/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose
```
���´ε�¼ʱ��Completion�����Ѿ�����ʹ��
���õĲ�ȫȡ�����������е����룬�Ჹȫ��  
* ���õ�docker-compose����  
* ����ĳһ�ر�������õ�ѡ��  
* ��һ�����������������������磺�������л�ֹͣ״̬��ʵ���ķ�����߻��ھ���ķ��� VS ����Dockerfile�ķ����£������п��еķ������ƣ�����`docker-compose scale`����ȫ��������ʱ���Զ���"="������ȥ  
* ���ڿ�ѡ��Ĳ��������磺`docker-compose kill -s`�����һЩ�źţ�����SIGUP��SIGUSR1  

> ӵ����������Ժ�Compose��������������أ�Happy working��

----------------------------------------------
### ����Composeʹ��ʵ��    
�ڱ����н���ʵ������nginx����һ�����ݾ����������������ݾ�������Ϊnginx�ľ�̬�ļ�  
1.����compose�ļ���  
`sudo mkdir composetest`  
`cd composetest`  
2.����docker-compose.yml�ļ�  
`touch docker-compose.yml`  
`vim docker-compose.yml`  
��docker-compose.yml�������������ݣ�  
```
dvc:  
  image: debian:wheezy
  volumes:  
   - /www:/usr/share/nginx/html:ro

nginx:
  image: nginx:latest
  volumes_from:
   - dvc
  ports:
   - "8081:80"
```  
3.����  
`docker-compose up -d`  
ע��ʹ������`docker-compose ps`�鿴����״�� 
 
-----------------------------------------
### �ġ�CLI ˵����docker-compose ���
�����Compose�����������һ����������ģ��������û��ָ�����������Ӧ�õ����з������Ҫ������п�����Ϣ��ʹ�����`docker-compose [COMMAND] --help`�����������COMMAND����˵���� 
 
 build  
���������ٽ�����  
���񱻴��������Ϊproject_service(����composetest_db)������ı���һ�������Dockerfile���߹���Ŀ¼�����ݣ�����ʹ��`docker-compose build`���ؽ���  

help  
��ʾ����İ�����ʹ����Ϣ  

kill  
ͨ������`SIGKILL`���ź�ǿ��ֹͣ���е�����������źſ���ѡ���Ե�ͨ�������磺  
`docker-compose kill -s SIGKINT`  

logs  
��ʾ�������־���  

port  
Ϊ�˿ڰ����������Ϣ  

ps  
��ʾ����  

pull  
��ȡ������  

rm  
ɾ��ֹͣ������  

run  
�ڷ���������һ��һ����������磺  
`docker-compose run web python manage.py shell`

scale  
����Ϊһ�����������������������������������Ĳ�����ʽָ���ģ�service=num�����磺 
`docker-compose scale web=2 worker=3`  

start  
�����Ѿ����ڵ�������Ϊһ������  
  
stop  
ֹͣ���е���������ɾ�����ǣ����ǿ���ʹ������`docker-compose start`������������  

up  
Ϊһ�����񹹽������������������ӵ�����  
���ӵķ���ᱻ���������������Ѿ���������  
Ĭ������£�`docker-compose up`�Ἧ��ÿ�������������������ʱ�����е�������ֹͣ������`docker-compose up -d`���ں�̨����������ʹ��������  
Ĭ������£����������������Ļ���`docker-compose up`��ֹͣ���ٴ������ǣ�ʹ����volumes-from�ᱣ���ѹ��صľ����������ʹ����ֹͣ���ٴ����Ļ���ʹ��`docker-compose up --no-recreate`���������Ҫ�Ļ�����������κ�ֹͣ������  

#### **ѡ��**  
--verbose  
��ʾ�������  

--version  
��ʾ�汾�Ų��˳�  

-f,--file FILE  
ָ��һ����ѡ��Compose yaml�ļ���Ĭ�ϣ�docker-compose.yml��

-p,--project-name NAME  
ָ����ѡ����Ŀ���ƣ�Ĭ�ϣ���ǰĿ¼���ƣ�  



-----------------------------------------
### �塢docker-compose.yml����˵��  
ÿһ��������docker-compose.yml�еķ��������ȷָ��һ��image����buildѡ�����`docker run`��������������Ƕ�Ӧ��ͬ�ģ�����`docker run`����Dockerfile�ļ���ָ����ѡ�����CMD��EXPOSE��VOLUME��ENV����Ĭ�ϵģ���˲�����docker-compose.yml����ָ��һ��  
  
image  
����image��ID�����image ID�����Ǳ���Ҳ������Զ�̵ģ�������ز����ڣ�Compose�᳢��ȥpull����  
```  
image: ubuntu  
image: orchardup/postgresql  
image: a4bc65fd  
```

build  
�ò���ָ��Dockerfile�ļ���·������Ŀ¼Ҳ�Ƿ��͵��ػ����̵Ĺ�������������е㣩��Compose������һ���Ѵ��ڵ����ƽ��й�������ǣ������ʹ�����image  
```  
build: /path/to/build/dir  
```  

command  
��дĬ�ϵ�����  
```  
command: bundle exec thin -p 3000  
```  

links  
���ӵ����������е�����������ָ���������ƺ�������ӵı���������ָֻ����������  
```  
links:  
 - db  
 - db:database  
 - redis  
```  
��ʱ���������ڲ�������`/etc/hosts`�ļ����ñ�������һ����Ŀ������������  
```  
172.17.2.186  db  
172.17.2.186  database  
172.17.2.186  redis  
```  
��������Ҳ�ᱻ���������ڻ��������Ĳ��������ں��潲��  

external_links  
���ӵ������docker-compose.yml�ļ�����Compose�ⲿ�������������ر��Ƕ����ṩ����͹����������������ָ���������ƺͱ���ʱ��`external_links`��ѭ�ź�`links`��ͬ�������÷�  
```  
external_links:  
 - redis_1  
 - project_db_1:mysql  
 - project_db_1:postgresql  
```  

ports  
��¶�˿ڣ�ָ�����ߵĶ˿ڣ�������������������ֻ�������Ķ˿ڣ������ᱻ�������һ���˿ڣ�  
> ע������ ����:���� ����ʽ��ӳ��˿�ʱ�����ʹ�����Ķ˿�С��60���ǿ��ܻ���ִ�����ΪYAML�Ὣ xx:yy������ʽ�����ݽ���Ϊ��ʮ���Ƶ����ݣ��������ԭ��ʱ�̼ǵ�Ҫ���˿�ӳ����ȷָ��Ϊ�ַ���  

```  
ports:  
 - "3000"  
 - "8000:8000"  
 - "49100:22"  
 - "127.0.0.1:8001:8001"  
```  

expose  
��¶�˿ڶ������������������ǣ���ֻ�ǻ������ӵķ���linked service���ṩ��ֻ���ڲ��˿ڿ��Ա�ָ��  
```  
expose:  
 - "3000"  
 - "8000"  
```  

volumes  
����·����Ϊ������ѡ���Ե�ָ��һ�������ϵ�·����������������������һ�ֿ�ʹ�õ�ģʽ��������������ro��  
```  
volumes_from:  
 - service_name  
 - container_name  
```  

environment  
���뻷������������ʹ����������ֵ䣬ֻ��һ��key�Ļ�����������������Compose�Ļ������ҵ���Ӧ��ֵ���������ڼ��ܵĻ�������������ֵ  
```  
environment:  
  RACK_ENV: development  
  SESSION_SECRET:  
environments:  
  - RACK_ENV=development  
  - SESSION_SECRET  
```  

env_file  
��һ���ļ��м��뻷�����������ļ�������һ��������ֵ����һ���б���`environment`��ָ���Ļ�������������д��Щֵ  
```  
env_file:  
  - .env  


RACK_ENV: development  
```  

net  
����ģʽ��������docker�ͻ��˵�`--net`������ָ����Щֵ  
```  
net: "bridge"  
net: "none"  
net: "container:[name or id]"  
net: "host"  
```  

dns  
�Զ���DNS���񣬿�����һ��������ֵ����һ���б�  
```  
dns: 8.8.8.8  
dns:  
  - 8.8.8.8  
  - 9.9.9.9  
```  

cap_add,cap_drop  
�������ȥ�������������鿴`man 7 capabilities` ������һ���������б�  
```  
cap_add:
  - ALL  

cap_drop:  
  - NET_ADMIN  
  - SYS_ADMIN  
```  

dns_search  
�Զ���DNS������Χ�������ǵ�����ֵ����һ���б�  
```
dns_search: example.com  
dns_search:  
  - dc1.example.com  
  - dc2.example.com  
```  

working_dir,entrypoint,user,hostname,domainname,mem_limit,privileged,restart,stdin_open,tty,cpu_shares  
������ÿһ����ֻ��һ��������ֵ����`docker run`�ж�Ӧ�Ĳ�����һ����  
```  
cpu_shares: 73

working_dir: /code
entrypoint: /code/entrypoint.sh
user: postgresql

hostname: foo
domainname: foo.com

mem_limit: 1000000000
privileged: true

restart: always

stdin_open: true
tty: true  
```

-------------------------------
### ����Compose��������˵��  
���������Ѿ��������������ӷ�����Ƽ������ˣ��෴��Ӧ��ʹ���������ƣ�Ĭ������������ӷ�������ƣ���Ϊ�������������ӣ�����Բ鿴`docker-compose.yml`�ĸ���ϸ��  
Composeʹ��Docker links����¶����������������ġ�ÿһ�����ӵ�������ʹ����һ�黷����������ÿһ�黷�������������������ƵĴ�д��ĸ��ͷ��  
Ҫ�鿴������õĻ�������������`docker-compose run SERVICE env`  

name_PORT  
����URL���磺DB_PORT=tcp//172.17.0.5:5432  

name_PORT_num_protocol  
����URL���磺DB_PORT_5432_TCP=tcp://172.17.0.5:5432  

name_PORT_num_protocol_ADDR  
������IP��ַ���磺DB_PORT_5432_TCP_ADDR=172.17.0.5  

name_PORT_num_protocol_PORT
��¶�Ķ˿ںţ��磺DB_PORT_5432_TCP_PORT=5432  

name_PORT_num_protocol_PROTO  
Э��(tcp����udp)���磺DB_PORT_5432_TCP_PROTO=tcp  

name_NAME  
��ȫ�ϸ���������ƣ��磺DB_1_NAME=/myapp_web_1/myapp_db_1  
