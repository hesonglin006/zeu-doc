### 配置Nginx Docker作为静态文件服务器 ###
**方法1.直接使用命令行**  
1.首先要将 nginx 给pull下来  
`sudo docker pull nginx`  
2.将 debian:wheezy 给pull下来
执行该步的原因是因为考虑到后面实现数据卷容器共享时的相通性。  
`sudo dcoker pull debian:wheezy`  
3.启动一个数据卷容器  
`sudo docker run --name data-volume-container -v /www:/usr/share/nginx/html:ro -d debian:wheezy`  
* 其中/usr/share/nginx/html是nginx的默认文件目录  
* 以debian:wheezy来启动是为了保证和nginx的目录结构相同（具体可查看nginx的Dockerfile文件）,当然，使用Ubuntu也可以，其具有相同的目录结构

4.然后启动nginx，注意各个参数  
  
`docker run --volumes-from data-volume-container --name nginx-server -p 80:80 -d nginx`  
注：  
* 访问主机的默认端口即可访问到nginx

---------------------------------

最后，只需要在浏览器中输入`http://localhost`或者`http://host-ip`就可进行访问  

---------------------------------  

附：nginx 的 Dockerfile：  
`FROM debian:wheezy`
`MAINTAINER NGINX Docker Maintainers "docker-maint@nginx.com"`
`RUN apt-key adv --keyserver pgp.mit.edu --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62`
`RUN echo "deb http://nginx.org/packages/mainline/debian/ wheezy nginx" >> /etc/apt/sources.list`
`ENV NGINX_VERSION 1.7.11-1~wheezy`
`RUN apt-get update && \
    apt-get install -y ca-certificates nginx=${NGINX_VERSION} && \
    rm -rf /var/lib/apt/lists/*`  
`\# forward request and error logs to docker log collector`
`RUN ln -sf /dev/stdout /var/log/nginx/access.log`  
`RUN ln -sf /dev/stderr /var/log/nginx/error.log`
`VOLUME ["/var/cache/nginx"]`
`EXPOSE 80 443`
`CMD ["nginx", "-g", "daemon off;"]`
