### ����Nginx Docker��Ϊ��̬�ļ������� ###
**����1.ֱ��ʹ��������**  
1.����Ҫ�� nginx ��pull����  
`sudo docker pull nginx`  
2.�� debian:wheezy ��pull����
ִ�иò���ԭ������Ϊ���ǵ�����ʵ�����ݾ���������ʱ����ͨ�ԡ�  
`sudo dcoker pull debian:wheezy`  
3.����һ�����ݾ�����  
`sudo docker run --name data-volume-container -v /www:/usr/share/nginx/html:ro -d debian:wheezy`  
* ����/usr/share/nginx/html��nginx��Ĭ���ļ�Ŀ¼  
* ��debian:wheezy��������Ϊ�˱�֤��nginx��Ŀ¼�ṹ��ͬ������ɲ鿴nginx��Dockerfile�ļ���,��Ȼ��ʹ��UbuntuҲ���ԣ��������ͬ��Ŀ¼�ṹ

4.Ȼ������nginx��ע���������  
  
`docker run --volumes-from data-volume-container --name nginx-server -p 80:80 -d nginx`  
ע��  
* ����������Ĭ�϶˿ڼ��ɷ��ʵ�nginx

---------------------------------

���ֻ��Ҫ�������������`http://localhost`����`http://host-ip`�Ϳɽ��з���  

---------------------------------  

����nginx �� Dockerfile��  
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
