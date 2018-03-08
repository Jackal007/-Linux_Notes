# ncrack

ncrack是一个网络认证破解的开源工具。它是专为高速平行开裂使用动态引擎，可以适应不同的网络情况。对于特殊情况，Ncrack也可以进行广泛的微调，尽管默认参数通用性足以涵盖几乎所有情况。它建立在模块化架构上，可以轻松扩展以支持其他协议。Ncrack专为企业和安全专业人员以快速可靠的方式审核大型网络的默认或弱密码。它也可以用来对个别服务进行相当复杂和强烈的暴力攻击。

Ncrack的输出是为每个指定的目标找到的证书列表（如果有的话）。Ncrack还可以打印到目前为止的交互式状态报告，如果用户选择了该选项，可能还会附加一些调试信息来帮助跟踪问题。



**模块：**

FTP，SSH，Telnet，HTTP（S），POP3（S），SMB，RDP，VNC，SIP，Redis，PostgreSQL，MySQL

## 用法：

ncrack \[Options\] {target and service specification}

ncrack \[选项\] {目标和服务规格}



**目标规格：**

可以通过主机名，IP地址，网络等

例如：scanme.nmap.org，microsoft.com/24,192.168.0.1; 10.0.0-255.1-254

-iX &lt;输入文件名&gt;：从Nmap的-oX XML输出格式输入

-iN &lt;输入文件名&gt;：从Nmap的-oN正常输出格式输入

-iL &lt;输入文件名&gt;：从主机/网络列表中输入

--exclude&lt;host1 \[，host2\] \[，host3\]，...&gt;：排除主机/网络

--excludefile &lt;exclude\_file&gt;：从文件中排除列表



**服务规格：**

可以通过&lt;service&gt;：// target（标准）表示法或者使用-p将以非标准符号应用于所有主机。

可以将服务参数指定为主机特定的，特定于服务的类型（-m）或全局（-g）。

例如：ssh：//10.0.0.10,at=10,cl=30 -m ssh：at = 50 -g cd = 3000

例2：ncrack -p ssh，ftp：3500,25 10.0.0.10 scanme.nmap.org google.com:80,ssl

-p &lt;服务列表&gt;：服务将被应用于所有非标准的符号主机

-m &lt;服务&gt;：&lt;选项&gt;：选项将应用于此类型的所有服务

-g &lt;选项&gt;：选项将应用于全局的每个服务



**其他选项：**

ssl：通过此服务启用SSL

path &lt;name&gt;：用于模块像HTTP（'='需要转义，如果使用）



**时间和性能：**

采取&lt;time&gt;的选项是以秒为单位的，除非你附加'ms'  （毫秒），“m”（分钟）或“h”（小时）的值（例如30m）。



特定于服务的选项：

cl（最小连接限制）：最小并发并行连接数

CL（最大连接限制）：并行并行连接的最大数量

at（身份验证尝试）：每个连接的身份验证尝试

cd（连接延迟）：每个连接启动之间的延迟&lt;time&gt;

cr（连接重试）：服务连接尝试次数上限

to（超时）：服务的最大开发&lt;时间&gt;，不管迄今为止成功

-T &lt;0-5&gt;：设定时间模板（越快越快）

--connection-limit &lt;number&gt;：总并发连接的阈值



**认证：**

-U &lt;文件名&gt;：用户名文件

-P &lt;文件名&gt;：密码文件

--user &lt;username\_list&gt;：逗号分隔的用户名单

--pass &lt;password\_list&gt;：逗号分隔的密码列表

--passwords-first：迭代每个用户名的密码列表。默认是相反的。

--pairwise：成对选择用户名和密码。



**OUTPUT：**

-oN / -oX &lt;文件&gt;：分别以正常和XML格式输出扫描到给定的文件名。

-oA &lt;basename&gt;：一次输出两种主要格式

-v：增加详细程度（使用两次或更多效果更好）

-d \[级别\]：设置或增加调试级别（最多10个有意义）

--nsock-trace &lt;level&gt;：设置nsock跟踪级别（有效范围：0 - 10）

--log-errors：将错误/警告记录到正常格式的输出文件

--append-output：附加到指定的输出文件而不是clobber



**MISC：**

--resume &lt;file&gt;：继续先前保存的会话

--save&lt;file&gt;：保存具有特定文件名的恢复文件

-f：在找到一个证书后退出服务

-6：启用IPv6破解

-sL或--list：只列出主机和服务

--datadir &lt;dirname&gt;：指定自定义Ncrack数据文件位置

--proxy &lt;type：// proxy：port&gt;：通过socks4，4a，http进行连接。

-V：打印版本号

-h：打印此帮助摘要页面。

## 例子

```

ncrack -v --user root localhost：22  

ncrack -v -T5 https://192.168.0.1 
 
ncrack -v -iX〜/ nmap.xml -g CL = 5，to = 1h  

ncrack -v https://www.fujieace.com  

ncrack 10.0.0.130:21 192.168.1.2:22  
```



