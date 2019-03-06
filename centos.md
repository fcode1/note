设置网络：
1.  ip addr
2. cd /etc  sysconfig/ network-scripts/
5. ll
6. vi ifcfg-enp0s3 -->，no改为yes 
7. 重启网络service network restart

 8. yum install wget  安装wget
9.  wget +nodejs下载地址
    xz -d 解压一层（.xz后缀） tar -xf 加压最后一层（.tar）

10.  需要软连接到usr/bin的node目录下：
       ln -s ~/文件夹目录/bin/node  /usr/bin/node
       需要软连接到usr/bin的npm目录下：
       ln -s ~/文件夹目录/bin/npm  /usr/bin/npm
      


     