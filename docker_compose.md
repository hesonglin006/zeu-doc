### 一、Compose安装   
在安装compose之前，要确保已经安装了docker1.3或以上版本  
在Linux64位系统上安装compose：  
```
curl -L https://github.com/docker/compose/releases/download/1.1.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose 
chmod +x /usr/local/bin/docker-compose  
``` 
* 注：当然可以选择安装`command completion`（见二）  
*     `uname -s`和`uname -m`中的两个引号是键盘上ESC下面的那个按键   
此时，compose已经安装成功，使用命令`docker-compose --version`可以查看 
 
如果是在OS X系统上，则需要执行如下步骤（未亲测）：
 
---------------------------------------------- 
### 二、Compose命令补全
#### 安装命令补全  
确保bash completion已经安装，如果当前使用非最小安装的Linux，bash completion已经OK了，如果是在MAC上，可以使用`brew install bash-completion`来安装  
将completion脚本放在/etc/bash_completion.d/（在MAC上是/usr/local/etc/bash_completion.d/）  
```
curl -L https://raw.githubusercontent.com/docker/compose/1.1.0/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose
```
在下次登录时，Completion功能已经可以使用
可用的补全取决于在命令行的输入，会补全：  
* 可用的docker-compose命令  
* 对于某一特别命令可用的选项  
* 在一个给定的上下文条件（比如：具有运行或停止状态的实例的服务或者基于镜像的服务 VS 基于Dockerfile的服务）下，给出有可行的服务名称，对于`docker-compose scale`，补全服务名称时会自动有"="附加上去  
* 对于可选项的参数，比如：`docker-compose kill -s`会完成一些信号，比如SIGUP和SIGUSR1  

> 拥有了这项功能以后，Compose更快更少输入了呢！Happy working！

----------------------------------------------
### 三、Compose使用实例    
在本例中将会实现启动nginx服务及一个数据卷容器，并将该数据卷容器作为nginx的静态文件  
1.创建compose文件夹  
`sudo mkdir composetest`  
`cd composetest`  
2.创建docker-compose.yml文件  
`touch docker-compose.yml`  
`vim docker-compose.yml`  
在docker-compose.yml中输入以下内容：  
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
3.启动  
`docker-compose up -d`  
注：使用命令`docker-compose ps`查看运行状况 
 
-----------------------------------------
### 四、CLI 说明（docker-compose 命令）
大多数Compose命令都是运行于一个或多个服务的，如果服务没有指定，该命令将会应用到所有服务，如果要获得所有可用信息，使用命令：`docker-compose [COMMAND] --help`，下面是命令（COMMAND）的说明： 
 
 build  
创建或者再建服务  
服务被创建后会标记为project_service(比如composetest_db)，如果改变了一个服务的Dockerfile或者构建目录的内容，可以使用`docker-compose build`来重建它  

help  
显示命令的帮助和使用信息  

kill  
通过发送`SIGKILL`的信号强制停止运行的容器，这个信号可以选择性的通过，比如：  
`docker-compose kill -s SIGKINT`  

logs  
显示服务的日志输出  

port  
为端口绑定输出公共信息  

ps  
显示容器  

pull  
拉取服务镜像  

rm  
删除停止的容器  

run  
在服务上运行一个一次性命令，比如：  
`docker-compose run web python manage.py shell`

scale  
设置为一个服务启动的容器数量，数量是以这样的参数形式指定的：service=num，比如： 
`docker-compose scale web=2 worker=3`  

start  
启动已经存在的容器作为一个服务  
  
stop  
停止运行的容器而不删除它们，它们可以使用命令`docker-compose start`重新启动起来  

up  
为一个服务构建、创建、启动、附加到容器  
连接的服务会被启动，除非它们已经在运行了  
默认情况下，`docker-compose up`会集中每个容器的输出，当存在时，所有的容器会停止，运行`docker-compose up -d`会在后台启动容器并使它们运行  
默认情况下，如果服务存在容器的话，`docker-compose up`会停止并再创建它们（使用了volumes-from会保留已挂载的卷），如果不想使容器停止并再创建的话，使用`docker-compose up --no-recreate`，如果有需要的话，这会启动任何停止的容器  

#### **选项**  
--verbose  
显示更多输出  

--version  
显示版本号并退出  

-f,--file FILE  
指定一个可选的Compose yaml文件（默认：docker-compose.yml）

-p,--project-name NAME  
指定可选的项目名称（默认：当前目录名称）  



-----------------------------------------
### 五、docker-compose.yml命令说明  
每一个定义在docker-compose.yml中的服务必须明确指定一个image或者build选项，这与`docker run`命令行中输入的是对应相同的，对于`docker run`，在Dockerfile文件中指定的选项（比如CMD、EXPOSE、VOLUME、ENV）是默认的，因此不必在docker-compose.yml中再指定一次  
  
image  
标明image的ID，这个image ID可以是本地也可以是远程的，如果本地不存在，Compose会尝试去pull下来  
```  
image: ubuntu  
image: orchardup/postgresql  
image: a4bc65fd  
```

build  
该参数指定Dockerfile文件的路径，该目录也是发送到守护进程的构建环境（这句有点），Compose将会以一个已存在的名称进行构建并标记，并随后使用这个image  
```  
build: /path/to/build/dir  
```  

command  
重写默认的命令  
```  
command: bundle exec thin -p 3000  
```  

links  
连接到其他服务中的容器，可以指定服务名称和这个链接的别名，或者只指定服务名称  
```  
links:  
 - db  
 - db:database  
 - redis  
```  
此时，在容器内部，会在`/etc/hosts`文件中用别名创建一个条目，就像这样：  
```  
172.17.2.186  db  
172.17.2.186  database  
172.17.2.186  redis  
```  
环境变量也会被创建，关于环境变量的参数，会在后面讲到  

external_links  
连接到在这个docker-compose.yml文件或者Compose外部启动的容器，特别是对于提供共享和公共服务的容器。在指定容器名称和别名时，`external_links`遵循着和`links`相同的语义用法  
```  
external_links:  
 - redis_1  
 - project_db_1:mysql  
 - project_db_1:postgresql  
```  

ports  
暴露端口，指定两者的端口（主机：容器），或者只是容器的端口（主机会被随机分配一个端口）  
> 注：当以 主机:容器 的形式来映射端口时，如果使容器的端口小于60，那可能会出现错误，因为YAML会将 xx:yy这样格式的数据解析为六十进制的数据，基于这个原因，时刻记得要将端口映射明确指定为字符串  

```  
ports:  
 - "3000"  
 - "8000:8000"  
 - "49100:22"  
 - "127.0.0.1:8001:8001"  
```  

expose  
暴露端口而不必向主机发布它们，而只是会向链接的服务（linked service）提供，只有内部端口可以被指定  
```  
expose:  
 - "3000"  
 - "8000"  
```  

volumes  
挂载路径最为卷，可以选择性的指定一个主机上的路径（主机：容器），或是一种可使用的模式（主机：容器：ro）  
```  
volumes_from:  
 - service_name  
 - container_name  
```  

environment  
加入环境变量，可以使用数组或者字典，只有一个key的环境变量可以在运行Compose的机器上找到对应的值，这有助于加密的或者特殊主机的值  
```  
environment:  
  RACK_ENV: development  
  SESSION_SECRET:  
environments:  
  - RACK_ENV=development  
  - SESSION_SECRET  
```  

env_file  
从一个文件中加入环境变量，该文件可以是一个单独的值或者一张列表，在`environment`中指定的环境变量将会重写这些值  
```  
env_file:  
  - .env  


RACK_ENV: development  
```  

net  
网络模式，可以在docker客户端的`--net`参数中指定这些值  
```  
net: "bridge"  
net: "none"  
net: "container:[name or id]"  
net: "host"  
```  

dns  
自定义DNS服务，可以是一个单独的值或者一张列表  
```  
dns: 8.8.8.8  
dns:  
  - 8.8.8.8  
  - 9.9.9.9  
```  

cap_add,cap_drop  
加入或者去掉容器能力，查看`man 7 capabilities` 可以有一张完整的列表  
```  
cap_add:
  - ALL  

cap_drop:  
  - NET_ADMIN  
  - SYS_ADMIN  
```  

dns_search  
自定义DNS搜索范围，可以是单独的值或者一张列表  
```
dns_search: example.com  
dns_search:  
  - dc1.example.com  
  - dc2.example.com  
```  

working_dir,entrypoint,user,hostname,domainname,mem_limit,privileged,restart,stdin_open,tty,cpu_shares  
上述的每一个都只是一个单独的值，和`docker run`中对应的参数是一样的  
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
### 六、Compose环境变量说明  
环境变量已经不再是用来连接服务的推荐方法了，相反，应该使用链接名称（默认情况下是链接服务的名称）作为主机名称来连接，这可以查看`docker-compose.yml`的更多细节  
Compose使用Docker links来暴露服务的容器给其他的。每一个链接的容器都使用了一组环境变量，这每一组环境变量都是以容器名称的大写字母开头的  
要查看服务可用的环境变量，运行`docker-compose run SERVICE env`  

name_PORT  
完整URL，如：DB_PORT=tcp//172.17.0.5:5432  

name_PORT_num_protocol  
完整URL，如：DB_PORT_5432_TCP=tcp://172.17.0.5:5432  

name_PORT_num_protocol_ADDR  
容器的IP地址，如：DB_PORT_5432_TCP_ADDR=172.17.0.5  

name_PORT_num_protocol_PORT
暴露的端口号，如：DB_PORT_5432_TCP_PORT=5432  

name_PORT_num_protocol_PROTO  
协议(tcp或者udp)，如：DB_PORT_5432_TCP_PROTO=tcp  

name_NAME  
完全合格的容器名称，如：DB_1_NAME=/myapp_web_1/myapp_db_1  
