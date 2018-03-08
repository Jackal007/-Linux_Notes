hydra用法：  

hydra &lt;参数&gt; &lt;IP地址&gt; &lt;服务名&gt;  

hydra的参数是区分大小写的.  

hydra支持的服务有:telnet ftp ssh mysql mssql vnc pcanywhere RDP\(3389\)等.  

hydra的一些参数:  

-R 继续从上一次的进度开始爆破  

-s &lt;port&gt; 指定端口  

-l &lt;username&gt; 指定登录的用户名  

-L &lt;username-list&gt; 指定用户名字典  

-p &lt;password&gt; 指定密码  

-t &lt;number&gt; 设置线程数  

-P &lt;passwd-list&gt; 指定密码字典  

-v 显示详细过程  

  

示例:  

hydra -l root -P /tmp/passwd.txt -t 4 -v 192.168.57.101 ssh \#爆破ssh登录密码 



