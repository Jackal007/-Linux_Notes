### hydra用法：

hydra &lt;参数&gt; &lt;IP地址&gt; &lt;服务名&gt;

> hydra的参数是区分大小写的.
>
> hydra支持的服务有:telnet ftp ssh mysql mssql vnc pcanywhere RDP\(3389\)等.

hydra的一些参数:

* -R 
   继续从上一次的进度开始爆破
   
* -S  
   大写，采用SSL链接
   
* -s &lt;port&gt; 
   指定端口
   
* -l &lt;username&gt; 
   指定登录的用户名
   
* -L &lt;username-list&gt; 
   指定用户名字典
   
* -p &lt;password&gt; 
   指定密码
   
* -P &lt;passwd-list&gt; 
   指定密码字典
   
* -t &lt;number&gt; 
   设置线程数
   
* -e  
   可选选项，n：空密码试探，s：使用指定用户和密码试探

* -C 
   使用冒号分割格式，例如“登录名:密码”来代替-L/-P参数

* -M &lt;FILE&gt;  
   指定目标列表文件一行一条

* -o &lt;FILE&gt;  
   指定结果输出文件

* -f  
   在使用-M参数以后，找到第一对登录名或者密码的时候中止破解

* -t &lt;TASKS&gt;  
   同时运行的线程数，默认为16

* -w &lt;TIME&gt;  
   设置最大超时的时间，单位秒，默认是30s

* -v / -V  
   显示详细过程



示例:

hydra -l root -P /tmp/passwd.txt -t 4 -v 192.168.57.101 ssh \#爆破ssh登录密码

