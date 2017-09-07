### 观察目前的背景工作状态： jobs

```
[root@study ~]# jobs [-lrs]
选项与参数：
-l ：除了列出 job number 与指令串之外，同时列出 PID 的号码；
-r ：仅列出正在背景 run 的工作；
-s ：仅列出正在背景当中暂停 (stop) 的工作。


范例一：观察目前的 bash 当中，所有的工作，与对应的 PID
[root@study ~]# jobs -l
[1]- 14566 Stopped vim ~/.bashrc
[2]+ 14567 Stopped find / -print
```

+ 代表最近被放到背景的工作号码， - 代表最近最后第二个被放置到背景中的工作号码。 而 超过最后第三个以后的工作，就不会有 +/- 符号存在了！

---

### 直接将指令丢到后台中『执行』的 &

```
[root@study ~]# xxxx &    #在指令后面加 & 表示当前命令在后台执行
[1] 14432
```

但是stdout和stderr会被显示到屏幕上，可以用数据重定向处理这个问题

###  将『目前』的工作丢到背景中『暂停』：\[ctrl\]-z

---

###  将背景工作拿到前景来处理：fg \(foreground\)

```
[root@study ~]# fg %jobnumber
选项与参数：
%jobnumber ：jobnumber 为工作号码(数字)。注意，那个 % 是可有可无的！
如果没有加参数，则是取出 [n]+ 的那个进程
```

##  让工作在背景下的状态变成运作中： bg

```
[root@study ~]# bg %jobnumber
选项与参数：
%jobnumber ：jobnumber 为工作号码(数字)。注意，那个 % 是可有可无的！
如果没有加参数，则是取出 [n]+ 的那个进程
```

---

###  杀死背景当中的工作： kill

```
[root@study ~]# kill -signal %jobnumber
[root@study ~]# kill -l
选项与参数：
-l ：这个是 L 的小写，列出目前 kill 能够使用的讯号 (signal) 有哪些？
signal ：代表给予后面接的那个工作什么样的指示啰！用 man 7 signal 可知：
 -1 ：重新读取一次参数的配置文件 (类似 reload)；
 -2 ：代表与由键盘输入 [ctrl]-c 同样的动作；
 -9 ：立刻强制删除一个工作；
 -15：以正常的进程方式终止一项工作。与 -9 是不一样的。
 
 范例：
 [root@study ~]# kill -SIGTERM %1
# -SIGTERM 与 -15 是一样的！您可以使用 kill -l 来查阅！
```

---

### 脱机管理 nohup

bash退出后，依然运行

```
[root@study ~]# nohup [指令与参数] <==在终端机前景中工作
[root@study ~]# nohup [指令与参数] & <==在终端机背景中工作
```



