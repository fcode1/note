lscpu 查看CPU属性
top 查看当前机器资源占用情况
df ：df-h查看磁盘空间
       df-i查看磁盘索引空间
ps aux  查看启动的服务
ps aux |grep “服务名”  查看具体启动的服务
systemctl disable firewalld    关闭防火墙后重启

useradd+名字   创建用户
cat /etc/passwd   查看所有用户
passwd 名字   给用户添加密码
ssh+用户名+ip地址   用其他用户名登录


groupadd+名字  创建用户组
cat /etc/group     查看用户组

useradd -G+用户组名+用户名   创建用户名并放在用户组内
groupdel+组名  删除用户组
userdel+用户名  删除用户
groups 用户名    查看属于哪个组
whoami  查看当前用户属于哪个组

usermod -g 组名 用户名   修改组内用户
su+用户名 切换用户
sudu su  切换回根用户
exit 退出当前用户

