## 用法：

cewl \[OPTION\] ... URL

cewl \[选项\] ... URL

--help，-h：显示帮助

--keep，-k：保留下载的文件

--depth x，-d x：深度到蜘蛛，默认2

--min\_word\_length，-m：最小字长，默认为3

--offsite，-o：让蜘蛛访问其他网站

--write，-w file：将输出写入文件

--ua，-u user-agent：用户代理发送

--no-words，-n：不输出单词表

--meta， -a 包含元数据

--meta\_file file：元数据的输出文件

--email，-e包括电子邮件地址

--email\_file file：电子邮件地址的输出文件

--meta-temp-dir directory：exiftool在解析文件时使用的临时目录，默认为/ tmp

--count，-c：显示找到的每个单词的计数



认证

--auth\_type：摘要或基本

--auth\_user：认证用户名

--auth\_pass：认证密码



代理支持

--proxy\_host：代理主机

--proxy\_port：代理端口，默认8080

--proxy\_username：代理的用户名，如果需要的话

--proxy\_password：代理的密码（如果需要）



Headers头

--header, -H: 格式名称：值 - 可以传递多个



--verbose, -v: 详细



URL: 蜘蛛网站。

