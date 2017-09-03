所谓这『人有失手，马有乱蹄』，您说是吧？所以啰，当然有的时候会『不小心手滑了一下』在 useradd 的时候加入了错误的设定数据。或者是，在使用 useradd 后，发现某些地方还可以进行细部修改。 此 时，当然我们可以直接到 /etc/passwd 或 /etc/shadow 去修改相对应字段的数据， 不过，Linux 也有 提供相关的指令让大家来进行账号相关数据的微调

```
[root@study ~]# usermod [-cdegGlsuLU] username
选项与参数：
-c ：后面接账号的说明，即 /etc/passwd 第五栏的说明栏，可以加入一些账号的说明。
-d ：后面接账号的家目录，即修改 /etc/passwd 的第六栏；
-e ：后面接日期，格式是 YYYY-MM-DD 也就是在 /etc/shadow 内的第八个字段数据
-f ：后面接天数，为 shadow 的第七字段。
-g ：后面接初始群组，修改 /etc/passwd 的第四个字段，亦即是 GID 的字段。
-G ：后面接次要群组，修改这个使用者能够支持的群组，修改的是 /etc/group 啰～
-a ：与 -G 合用，可『增加次要群组的支持』而非『设定』喔！
-l ：后面接账号名称。亦即是修改账号名称， /etc/passwd 的第一栏。。
-s ：后面接 Shell 的实际文件，例如 /bin/bash 或 /bin/csh 等等。
-u ：后面接 UID 数字啦！即 /etc/passwd 第三栏的资料；
-L ：暂时将用户的密码冻结，让他无法登入。其实仅改 /etc/shadow 的密码栏。
-U ：将 /etc/shadow 密码栏的 ! 拿掉。
```



