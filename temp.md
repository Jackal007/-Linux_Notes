## at：在某个时间点执行相关的程序或命令

```
[root@study ~]# at [-mldv] TIME
[root@study ~]# at -c 工作号码    #中间过程的输出会用邮件的方式发送给工作号码
```

选项与参数：  
-m ：当 at 的工作完成后，即使没有输出讯息，亦以 email 通知使用者该工作已完成。

-l ：at -l 相当于 atq，列出目前系统上面的所有该用户的 at 排程；

-d ：at -d 相当于 atrm ，可以取消一个在 at 排程中的工作；

-v ：可以使用较明显的时间格式栏出 at 排程中的任务栏表；

-c ：可以列出后面接的该项工作的实际指令内容。

```
范例一：再过五分钟后，新建一个文件
[root@study ~]# at now + 5 minutes #记得单位要加 s 喔！
at> touch /home/root/file #一行就是一个工作
at> <EOT> #这里输入 [ctrl] + d 就会出现 <EOF> 的字样！代表结束！
job 2 at Thu Jul 30 19:35:00 2015 # 说明第 2 个 at 工作将在 2015/07/30 的 19:35 进行！
# at 会进入 at shell 环境来执行命令，所以文件名什么的最好写绝对路径

范例二：将上述的第 2 项工作内容列出来查阅
[root@study ~]# at -c 2
#!/bin/sh #就是透过 bash shell 的啦！
# atrun uid=0 gid=0
# mail root 0
umask 22
....
cd /etc/cron\.d || {
 echo 'Execution directory inaccessible' >&2
 exit 1
}
${SHELL:-/bin/sh} << 'marcinDELIMITER410efc26'
touch /home/root/file # 这一行最重要！
marcinDELIMITER410efc26
# 你可以看到指令执行的目录 (/root)，还有多个环境变量与实际的指令内容啦！

范例三：由于机房预计于 2015/08/05 停电，我想要在 2015/08/04 23:00 关机？
[root@study ~]# at 23:00 2015-08-04
at> /bin/sync
at> /bin/sync
at> /sbin/shutdown -h now
at> <EOT>
job 3 at Tue Aug 4 23:00:00 2015
# 您瞧瞧！ at 还可以在一个工作内输入多个指令呢！不错吧！
```

> ```
> TIME：时间格式，这里可以定义出『什么时候要进行 at 这项工作』的时间，格式有：
>  HH:MM ex> 04:00
> 在今日的 HH:MM 时刻进行，若该时刻已超过，则明天的 HH:MM 进行此工作。
>  HH:MM YYYY-MM-DD ex> 04:00 2015-07-30
> 强制规定在某年某月的某一天的特殊时刻进行该工作！
>  HH:MM[am|pm] [Month] [Date] ex> 04pm July 30
> 也是一样，强制在某年某月某日的某时刻进行！
>  HH:MM[am|pm] + number [minutes|hours|days|weeks]
> ex> now + 5 minutes ex> 04pm + 3 days
> 就是说，在某个时间点『再加几个时间后』才进行。
> ```
>
> at 时会进入一个 at shell 的环境来让用户下达工作指令，此时，建议你最好使 用绝对路径来下达你的指令，比较不会有问题喔！
>
> 有些朋友会希望『我要在某某时刻，在我的终端机显示出 Hello 的字样』，然后就在 at 里面下达这 样的信息『 echo "Hello" 』。等到时间到了，却发现没有任何讯息在屏幕上显示，这是啥原因啊？ 这是因为 at 的执行与终端机环境无关，而所有 standard output/standard error output 都会传送到执行 者的 mailbox 去啦！所以在终端机当然看不到任何信息。那怎办？没关系， 可以透过终端机的装置 来处理！假如你在 tty1 登入，则可以使用『 echo "Hello" &gt; /dev/tty1 』来取代

##  batch：系统有空时才进行背景任务

用法同at



