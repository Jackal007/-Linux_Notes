### kill：杀死或者给进程一个讯号

```
[root@study ~]# kill -signal %jobnumber/PID    #jobsbumber前要加%，PID不用
[root@study ~]# kill -l
选项与参数：
-l ：列出目前 kill 能够使用的讯号 (signal) 有哪些
signal ：见下表
```

| 代号-名称 | 内容 |
| --- | --- |
| 1-SIGHUP | 启动被终止的进程，可让该 PID 重新读取自己的配置文件，类似重新启动 |
| 2-SIGINT | 相当于用键盘输入 \[ctrl\]-c 来中断一个进程的进行 |
| 9-SIGKILL | 代表强制中断一个进程的进行，如果该进程进行到一半， 那么尚未完成的部分可能会有『半产品』产生，类似 vim 会有 .filename.swp 保留下来。 |
| 15-SIGTERM | 以正常的结束进程来终止该进程。由于是正常的终止， 所以后续的动作会将他完成。不过，如果该进程已经发生问题，就是无法使用正常的方法终止时， 输入这个 signal 也是没有用的。 |
| 19-SIGSTOP | 相当于用键盘输入 \[ctrl\]-z 来暂停一个进程的进行 |

```
范例：
[root@study ~]# kill -SIGTERM %1
# -SIGTERM 与 -15 是一样的！您可以使用 kill -l 来查阅！
```

###  killall -signal 指令名称：杀死服务，所有以某个进程启动的进程都将被杀死

 由于 kill 后面必须要加上 PID \(或者是 job number\)，所以，通常 kill 都会配合 ps, pstree 等指令，因 为我们必须要找到相对应的那个进程的 ID 嘛！但是，如此一来，很麻烦～有没有可以利用『下达指 令的名称』来给予讯号的？举例来说，能不能直接将 rsyslogd 这个进程给予一个 SIGHUP 的讯号呢？ 可以的！用 killall 吧！

```
[root@study ~]# killall [-iIe] [command name]
选项与参数：
-i ：interactive 的意思，交互式的，若需要删除时，会出现提示字符给用户；
-e ：exact 的意思，表示『后面接的 command name 要一致』，但整个完整的指令
 不能超过 15 个字符。
-I ：指令名称(可能含参数)忽略大小写。

范例一：给予 rsyslogd 这个指令启动的 PID 一个 SIGHUP 的讯号
[root@study ~]# killall -1 rsyslogd
# 如果用 ps aux 仔细看一下，若包含所有参数，则 /usr/sbin/rsyslogd -n 才是最完整的！

范例二：强制终止所有以 httpd 启动的进程 (其实并没有此进程在系统内)
[root@study ~]# killall -9 httpd

范例三：依次询问每个 bash 程序是否需要被终止运作！
[root@study ~]# killall -i -9 bash
Signal bash(13888) ? (y/N) n <==这个不杀！
Signal bash(13928) ? (y/N) n <==这个不杀！
Signal bash(13970) ? (y/N) n <==这个不杀！
Signal bash(14836) ? (y/N) y <==这个杀掉！
# 具有互动的功能！可以询问你是否要删除 bash 这个程序。要注意，若没有 -i 的参数，
# 所有的 bash 都会被这个 root 给杀掉！包括 root 自己的 bash 喔！ ^_^
```



