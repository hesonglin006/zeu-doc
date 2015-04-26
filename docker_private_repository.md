### 在Ubuntu14.04上安装Docker
* 配置Docker的安装源  
        
`sudo apt-get update`  
`sudo apt-get install apt-transport-https`  
`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9`  
`sudo sh -c "echo deb https://get.docker.com/ubuntu docker main > /etc/apt/sources.list.d/docker.list"`
* 安装Docker  
  
`sudo apt-get update`  
`sudo apt-get install lxc-docker`  
* 启动Docker以及配置Docker开机启动  
  
`sudo apt-get install sysv-rc-conf`  
`sudo sysv-rc-conf docker on`  
`sudo service docker restart`    

-------------------------------------------
### Docker Registry server的部署（不带账号验证功能）
以下命令均在Ubuntu14.04系统上执行，使用如下命令可以创建docker registry server：  
> `docker run \
        --name registry-server \
        -d \
        -e SETTINGS_FLAVOR=local \
        -e SEARCH_BACKEND=sqlalchemy \
        -e STORAGE_PATH=/docker/registry \
        -v /docker/registry:/docker/registry \
        -p 192.168.0.112:80:5000 \
        registry`  

注释：  
--name   标记名称  
-d       在后台运行  
-e       设置运行的环境变量  
-v       把registry container里的image存储目录映射到主机上，有两点原因：  
* 因为存储是永久性的，如果把 image 存在 container 中，一旦 contaienr 给销毁，那么数据就没了。
* image 的文件系统必须有容量定义，一般是8G或者16G，所以一旦 image 多了，会导致主机磁盘还有空间，而 container 却报磁盘空间满，无法正常工作。不要在 container 里存放永久性数据。  
-p       registry 默认监听5000端口，而docker默认使用80端口，所以把container的5000端口映射到主机的80端口，免得在dockre push或者pull的时候加端口麻烦。  
 
### 对Docker private server的操作   
下面介绍向自己搭建的私有仓库进行pull push等操作的完整过程。  
* 操作环境：  
Docker private server位于ip为10.11.35.175（以下简称server）上，在子网的一台机器上安装ubuntu14.04 64位（以下简称client，client上已安装docker操作环境）进行对私有仓库的操作。  
  
* 操作步骤：  
1.在ubuntu14.04上pull下来一个image，本例中使用busybox(因为其很小)  
`sudo docker pull busybox`  
注：  
使用`sudo docker images`可以查看到现在本地所有的images  
使用`sudo docker ps`查看所有正在运行的container，如果要将未运行的也显示出来，则要加上参数`-a`  
2.将busybox运行起来并创建运行文件（类似对images进行添加新的功能）：  
`sudo docker run -it busybox /bin/sh`  
`/# pwd`  
`/`  
`/# vi /usr/bin/run.sh`  
内容为：
`#!/bin/sh
COUNT=0
while(true); do
        COUNT=$(($COUNT+2))
        echo $COUNT
        sleep 2
done`   
对该文件执行权限：  
`chmod 755 /usr/bin/run.sh`  
然后直接运行`run.sh`则会出现运行结果，即每隔2s中出现一个偶数。   
`+ C`可以退出脚本，再按`exit`或者`+ D`退出container，此时可以看到container处于exitted的状态，并且可以看到container的ID，比如为d20e。  
3.然后把该container提交为docker默认镜像仓库下，取名为zeu001/test，注意，此时只不过是一个标记，镜像只存在于本地，并未真正的传到镜像服务器，只能本地使用。  
`sudo docker commit d20e zeu001/test`  
4.对该镜像进行tag操作  
`sudo docker tag zeu001/test 10.11.35.175/zeu001/test`  
5.登录私有仓库   
> `sudo docker login 10.11.35.175`  
`Username:songlin`  
`Password: `
`Email:12345678@qq.com`  
`Login Succeeded`  

6.推送镜像到私有仓库  
`sudo docker push 10.11.35.175/zeu001/test`  
7.在浏览器中输入`http://10.11.35.175/v1/search`，可以看到仓库中所有的镜像文件。


