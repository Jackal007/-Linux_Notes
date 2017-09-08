### 直接将指令丢到后台中『执行』的 &

```
[root@study ~]# xxxx &    #在指令后面加 & 表示当前命令在后台执行
[1] 14432
```

但是stdout和stderr会被显示到屏幕上，可以用数据重定向处理这个问题

### 将『目前』的工作丢到背景中『暂停』：\[ctrl\]-z

---

### 将背景工作拿到前景来处理：fg \(foreground\)

```
[root@study ~]# fg %jobnumber
选项与参数：
%jobnumber ：jobnumber 为工作号码(数字)。注意，那个 % 是可有可无的！
如果没有加参数，则是取出 [n]+ 的那个进程
```

## 让工作在背景下的状态变成运作中： bg

```
[root@study ~]# bg %jobnumber
选项与参数：
%jobnumber ：jobnumber 为工作号码(数字)。注意，那个 % 是可有可无的！
如果没有加参数，则是取出 [n]+ 的那个进程
```

---

### 脱机管理 nohup

bash退出后，依然运行

```
[root@study ~]# nohup [指令与参数] <==在终端机前景中工作
[root@study ~]# nohup [指令与参数] & <==在终端机背景中工作
```



