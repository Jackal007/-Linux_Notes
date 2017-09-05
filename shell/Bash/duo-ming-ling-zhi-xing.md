###  ;

连续命令执行

```
#先将当前内容同步到磁盘两次，然后关机
[root@study ~]# sync; sync; shutdown -h now
```

---

> 命令执行结束后会把执行结果状态放到$?变量里面，
>
> 正确：$?=0
>
> 错误：$?≠0

### &&

 cmd1 && cmd2 

1. 若 cmd1 执行完毕且正确执行\($?=0\)，则开始执行 cmd2 

2. 若 cmd1 执行完毕且为错误 \($?≠0\)，则 cmd2 不执行

```
#使用 ls 查阅目录 /tmp/abc 是否存在，若存在则用 touch 建立 /tmp/abc/hehe
[dmtsai@study ~]$ ls /tmp/abc && touch /tmp/abc/hehe
ls: cannot access /tmp/abc: No such file or directory
# ls 很干脆的说明找不到该目录，但并没有 touch 的错误，表示 touch 并没有执行
```

### \|\|

 cmd1 \|\| cmd2 

1. 若 cmd1 执行完毕且正确执行\($?=0\)，则 cmd2 不执行

 2. 若 cmd1 执行完毕且为错误 \($?≠0\)，则开始执行 cmd2

```
#测试 /tmp/abc 是否存在，若不存在则予以建立，若存在就不作任何事情
[dmtsai@study ~]$ ls /tmp/abc || mkdir /tmp/abc
ls: cannot access /tmp/abc: No such file or directory <==真的不存在喔！
[dmtsai@study ~]$ ll -d /tmp/abc
drwxrwxr-x. 2 dmtsai dmtsai 6 Jul 9 19:17 /tmp/abca <==结果出现了！有进行 mkdi
```

---

```
#我不清楚 /tmp/abc 是否存在，但就是要建立 /tmp/abc/hehe 文件
[dmtsai@study ~]$ ls /tmp/abc || mkdir /tmp/abc && touch /tmp/abc/hehe
```



