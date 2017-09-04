### 优点：

##### 1.命令编修能力 \(history\)：

* 能记忆使用过的指令:

  ```
     ~/.bash\_history 记录的是前一次登入以前所执行过的指令， 而至于这一次登入所执行的指令都被暂存在内存中，当你成功的注  销系统后，该指令记忆才会记录到 .bash\_history 当中！
  ```

##### 2. 命令与文件补全功能： \(\[tab\] 按键的好处\)

##### 3. 命令别名设定功能： \(alias\)

```
alias lm='ls -al'    #设置自定义命令lm来取代ls -al
```

##### 4. 工作控制、前景背景控制： \(job control, foreground, background\)

##### 5. 程序化脚本： \(shell scripts\)

.sh脚本程序

##### 6. 通配符： \(Wildcard\)

相当于正则表达式

---

### 查询指令是bash指令，还是外部指令（非bash指令）

```
[dmtsai@study ~]$ type [-tpa] name
```

选项与参数：  
 ：不加任何选项与参数时，type 会显示出 name 是外部指令还是 bash 内建指令  
-t ：当加入 -t 参数时，type 会将 name 以底下这些字眼显示出他的意义：  
  file ：表示为外部指令；  
  alias ：表示该指令为命令别名所设定的名称；  
  builtin ：表示该指令为 bash 内建的指令功能；  
-p ：如果后面接的 name 为外部指令时，才会显示完整文件名；  
-a ：会由 PATH 变量定义的路径中，将所有含 name 的指令都列出来，包含 alias

    范例一：查询一下 ls 这个指令是否为 bash 内建？
    [dmtsai@study ~]$ type ls
    ls is aliased to `ls --color=auto' <==未加任何参数，列出 ls 的最主要使用情况
    [dmtsai@study ~]$ type -t ls
    alias <==仅列出 ls 执行时的依据
    [dmtsai@study ~]$ type -a ls
    ls is aliased to `ls --color=auto' <==最先使用 aliase
    ls is /usr/bin/ls <==还有找到外部指令在 /bin/ls
    范例二：那么 cd 呢？
    [dmtsai@study ~]$ type cd
    cd is a shell builtin <==看到了吗？ cd 是 shell 内建指令


### 快捷操作

 \[ctrl\]+u/\[ctrl\]+k 分别是从光标处向前删除指令串 \(\[ctrl\]+u\) 及向后删除指令串 \(\[ctrl\]+k\)。 

\[ctrl\]+a/\[ctrl\]+e 分别是让光标移动到整个指令串的最前面 \(\[ctrl\]+a\) 或最后面 \(\[ctrl\]+e\)。



