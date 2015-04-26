### ��Ubuntu14.04�ϰ�װDocker
* ����Docker�İ�װԴ  
        
`sudo apt-get update`  
`sudo apt-get install apt-transport-https`  
`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9`  
`sudo sh -c "echo deb https://get.docker.com/ubuntu docker main > /etc/apt/sources.list.d/docker.list"`
* ��װDocker  
  
`sudo apt-get update`  
`sudo apt-get install lxc-docker`  
* ����Docker�Լ�����Docker��������  
  
`sudo apt-get install sysv-rc-conf`  
`sudo sysv-rc-conf docker on`  
`sudo service docker restart`    

-------------------------------------------
### Docker Registry server�Ĳ��𣨲����˺���֤���ܣ�
�����������Ubuntu14.04ϵͳ��ִ�У�ʹ������������Դ���docker registry server��  
> `docker run \
        --name registry-server \
        -d \
        -e SETTINGS_FLAVOR=local \
        -e SEARCH_BACKEND=sqlalchemy \
        -e STORAGE_PATH=/docker/registry \
        -v /docker/registry:/docker/registry \
        -p 192.168.0.112:80:5000 \
        registry`  

ע�ͣ�  
--name   �������  
-d       �ں�̨����  
-e       �������еĻ�������  
-v       ��registry container���image�洢Ŀ¼ӳ�䵽�����ϣ�������ԭ��  
* ��Ϊ�洢�������Եģ������ image ���� container �У�һ�� contaienr �����٣���ô���ݾ�û�ˡ�
* image ���ļ�ϵͳ�������������壬һ����8G����16G������һ�� image ���ˣ��ᵼ���������̻��пռ䣬�� container ȴ�����̿ռ������޷�������������Ҫ�� container �������������ݡ�  
-p       registry Ĭ�ϼ���5000�˿ڣ���dockerĬ��ʹ��80�˿ڣ����԰�container��5000�˿�ӳ�䵽������80�˿ڣ������dockre push����pull��ʱ��Ӷ˿��鷳��  
 
### ��Docker private server�Ĳ���   
����������Լ����˽�вֿ����pull push�Ȳ������������̡�  
* ����������  
Docker private serverλ��ipΪ10.11.35.175�����¼��server���ϣ���������һ̨�����ϰ�װubuntu14.04 64λ�����¼��client��client���Ѱ�װdocker�������������ж�˽�вֿ�Ĳ�����  
  
* �������裺  
1.��ubuntu14.04��pull����һ��image��������ʹ��busybox(��Ϊ���С)  
`sudo docker pull busybox`  
ע��  
ʹ��`sudo docker images`���Բ鿴�����ڱ������е�images  
ʹ��`sudo docker ps`�鿴�����������е�container�����Ҫ��δ���е�Ҳ��ʾ��������Ҫ���ϲ���`-a`  
2.��busybox�������������������ļ������ƶ�images��������µĹ��ܣ���  
`sudo docker run -it busybox /bin/sh`  
`/# pwd`  
`/`  
`/# vi /usr/bin/run.sh`  
����Ϊ��
`#!/bin/sh
COUNT=0
while(true); do
        COUNT=$(($COUNT+2))
        echo $COUNT
        sleep 2
done`   
�Ը��ļ�ִ��Ȩ�ޣ�  
`chmod 755 /usr/bin/run.sh`  
Ȼ��ֱ������`run.sh`���������н������ÿ��2s�г���һ��ż����   
`+ C`�����˳��ű����ٰ�`exit`����`+ D`�˳�container����ʱ���Կ���container����exitted��״̬�����ҿ��Կ���container��ID������Ϊd20e��  
3.Ȼ��Ѹ�container�ύΪdockerĬ�Ͼ���ֿ��£�ȡ��Ϊzeu001/test��ע�⣬��ʱֻ������һ����ǣ�����ֻ�����ڱ��أ���δ�����Ĵ��������������ֻ�ܱ���ʹ�á�  
`sudo docker commit d20e zeu001/test`  
4.�Ըþ������tag����  
`sudo docker tag zeu001/test 10.11.35.175/zeu001/test`  
5.��¼˽�вֿ�   
> `sudo docker login 10.11.35.175`  
`Username:songlin`  
`Password: `
`Email:12345678@qq.com`  
`Login Succeeded`  

6.���;���˽�вֿ�  
`sudo docker push 10.11.35.175/zeu001/test`  
7.�������������`http://10.11.35.175/v1/search`�����Կ����ֿ������еľ����ļ���


