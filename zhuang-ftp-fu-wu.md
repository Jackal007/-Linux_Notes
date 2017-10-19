```bash
sudo apt-get install vsftpd

#新建一个文件夹用于FTP的工作目录
mkdir /home/ftp
给访问者这个文件夹权限（比如新建一个使用该文件夹的用户，或直接改权限就好）
    #新建用户uftp，制定用户主目录和所用shell，并设置密码    
    sudo useradd -d /home/ftp -s /bin/bash ftp
    sudo passwd ftp

#编辑VSFTPD配置文件 　 
VSFTPD配置文件为/etc/vsftpd.conf，
　　做如下修改： 
　　打开注释 write_enable=YES 
　　添加信息 userlist_file=/etc/vsftpd.user_list 
　　添加信息 userlist_enable=YES 
　　添加信息 userlist_deny=NO 
　　修改完成后保存退出。

#开启服务
service vsftpd start
```



