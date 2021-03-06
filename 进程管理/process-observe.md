> ```
> [root@study ~]# echo $$
> 14836 <==就是这个数字！他是我们 bash 的 PID
> ```

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

* 代表最近被放到背景的工作号码， - 代表最近最后第二个被放置到背景中的工作号码。 而 超过最后第三个以后的工作，就不会有 +/- 符号存在了！

## ps ：将某个时间点的进程运作情况撷取下来

```
[root@study ~]# ps aux    #观察系统所有的进程数据
[root@study ~]# ps -lA    #也是能够观察所有系统的数据
[root@study ~]# ps axjf    #连同部分进程树状态
选项与参数：
-A ：所有的 process 均显示出来，与 -e 具有同样的效用；
-a ：不与 terminal 有关的所有 process ；
-u ：有效使用者 (effective user) 相关的 process ；
x ：通常与 a 这个参数一起使用，可列出较完整信息。
输出格式规划：
l ：较长、较详细的将该 PID 的的信息列出；
j ：工作的格式 (jobs format)
-f ：做一个更为完整的输出。
```

* #### 仅观察自己的 bash 相关进程： ps -l

  ```
  [root@study ~]# ps -l
  F S UID PID PPID C PRI NI ADDR SZ WCHAN TTY TIME CMD
  4 S 0 14830 13970 0 80 0 - 52686 poll_s pts/0 00:00:00 sudo
  4 S 0 14835 14830 0 80 0 - 50511 wait pts/0 00:00:00 su
  4 S 0 14836 14835 0 80 0 - 29035 wait pts/0 00:00:00 bash
  0 R 0 15011 14836 0 80 0 - 30319 - pts/0 00:00:00 ps
  ```

  **各字段意思：**

  ##### F：代表这个进程旗标 \(process flags\)，说明这个进程的总结权限，常见号码有：

  若为 4 表示此进程的权限为 root ；  
  若为 1 则表示此子进程仅进行复制\(fork\)而没有实际执行\(exec\)。

  ##### S：代表这个进程的状态 \(STAT\)，主要的状态有：

  R \(Running\)：该程序正在运作中；  
  S \(Sleep\)：该程序目前正在睡眠状态\(idle\)，但可以被唤醒\(signal\)。  
  D ：不可被唤醒的睡眠状态，通常这支程序可能在等待 I/O 的情况\(ex&gt;打印\)  
  T ：停止状态\(stop\)，可能是在工作控制\(背景暂停\)或除错 \(traced\) 状态；  
  Z \(Zombie\)：僵尸状态，进程已经终止但却无法被移除至内存外。

  ##### UID/PID/PPID：代表『此进程被该 UID 所拥有/进程的 PID 号码/此进程的父进程 PID 号码』

  ##### C：代表 CPU 使用率，单位为百分比；

  ##### PRI/NI：Priority/Nice 的缩写，代表此进程被 CPU 所执行的优先级，数值越小代表该进程越快被 CPU 执行。

  ##### ADDR/SZ/WCHAN：都与内存有关，

  ADDR 是 kernel function，指出该进程在内存的哪个部分，如果是个running 的进程，一般就会显示『 - 』 / SZ 代表此进程用掉多少内存 / WCHAN 表示目前进程是否运作中，同样的， 若为 - 表示正在运作中。

  ##### TTY：登入者的终端机位置，若为远程登录则使用动态终端接口 \(pts/n\)；

  ##### TIME：使用掉的 CPU 时间，注意，是此进程实际花费 CPU 运作的时间，而不是系统时间；

  ##### CMD：就是 command 的缩写，造成此进程的触发程序之指令为何。

* #### 观察系统所有进程： ps aux

  ```
  [root@study ~]# ps aux
  USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
  root 1 0.0 0.2 60636 7948 ? Ss Aug04 0:01 /usr/lib/systemd/systemd ...
  root 2 0.0 0.0 0 0 ? S Aug04 0:00 [kthreadd]
  .....(中间省略).....
  root 14830 0.0 0.1 210744 3988 pts/0 S Aug04 0:00 sudo su -
  ```

  **各字段意思：**

  ##### USER：该 process 属于那个使用者账号的？

  ##### PID ：该 process 的进程标识符。

  ##### %CPU：该 process 使用掉的 CPU 资源百分比；

  ##### %MEM：该 process 所占用的物理内存百分比；

  ##### VSZ ：该 process 使用掉的虚拟内存量 \(Kbytes\)

  ##### RSS ：该 process 占用的固定的内存量 \(Kbytes\)

  ##### TTY ：该 process 是在那个终端机上面运作，

  若与终端机无关则显示 ?，另外， tty1-tty6 是本机上面的登入者进程，若为 pts/0 等等的，则表示为由网络连接进主机的进程。

  ##### STAT：该进程目前的状态，状态显示与 ps -l 的 S 旗标相同 \(R/S/T/Z\)

  ##### START：该 process 被触发启动的时间；

  ##### TIME ：该 process 实际使用 CPU 运作的时间。

  ##### COMMAND：该进程的实际指令为何？

> 僵尸进程：
>
> 通常，造成僵尸进程的成因是 因为该进程应该已经执行完毕，或者是因故应该要终止了， 但是该进程的父进程却无法完整的将 该进程结束掉，而造成那个进程一直存在内存当中。 如果你发现在某个进程的 CMD 后面还接上时，就代表该进程是僵尸进程。
>
> 当系统不稳定的时候就容易造成所谓的僵尸进程，可能是因为程序写的不好啦，或者是使用者的 操作习惯不良等等所造成。如果你发现系统中很多僵尸进程时，记得啊！要找出该进程的父进程， 然后好好的做个追踪，好好的进行主机的环境优化啊！ 看看有什么地方需要改善的，不要只是直 接将他 kill 掉而已呢！不然的话，万一他一直产生，那可就麻烦了！ @\_@

## top：动态观察进程的变化

```
[root@study ~]# top [-d 数字] | top [-bnp]
选项与参数：
-d ：后面可以接秒数，就是整个进程画面更新的秒数。预设是 5 秒；
-b ：以批次的方式执行 top ，还有更多的参数可以使用喔！
 通常会搭配数据流重导向来将批次的结果输出成为文件。
-n ：与 -b 搭配，意义是，需要进行几次 top 的输出结果。
-p ：指定某些个 PID 来进行观察监测而已。

在 top 执行过程当中可以使用的按键指令：
  ? ：显示在 top 当中可以输入的按键指令；
  P ：以 CPU 的使用资源排序显示；
  M ：以 Memory 的使用资源排序显示；
  N ：以 PID 来排序喔！
  T ：由该 Process 使用的 CPU 时间累积 (TIME+) 排序。
  k ：给予某个 PID 一个讯号 (signal)
  r ：给予某个 PID 重新制订一个 nice 值。
  q ：离开 top 软件的按键。
```

```
[root@study ~]# top

top - 00:53:59 up 6:07, 3 users, load average: 0.00, 0.01, 0.05
Tasks: 179 total, 2 running, 177 sleeping, 0 stopped, 0 zombie
%Cpu(s): 0.0 us, 0.0 sy, 0.0 ni,100.0 id, 0.0 wa, 0.0 hi, 0.0 si, 0.0 st
KiB Mem : 2916388 total, 1839140 free, 353712 used, 723536 buff/cache
KiB Swap: 1048572 total, 1048572 free, 0 used. 2318680 avail Mem
##########################################################
 PID USER PR NI VIRT RES SHR S %CPU %MEM TIME+ COMMAND
18804 root 20 0 130028 1872 1276 R 0.5 0.1 0:00.02 top
 1 root 20 0 60636 7948 2656 S 0.0 0.3 0:01.70 systemd
 2 root 20 0 0 0 0 S 0.0 0.0 0:00.01 kthreadd
 3 root 20 0 0 0 0 S 0.0 0.0 0:00.00 ksoftirqd/0
```

显示结果可以分为两个部分

* #### 上面的部分

  ##### 第一行\(top...\)：

  目前的时间，亦即是 00:53:59 那个项目；  
  开机到目前为止所经过的时间，亦即是 up 6:07, 那个项目；  
  已经登入系统的用户人数，亦即是 3 users, 项目；  
  系统在 1, 5, 15 分钟的平均工作负载。我们在第十五章谈到的 batch 工作方式为负载小于 0.8 就是这个负载啰！代表的是 1, 5, 15 分钟，系统平均要负责运作几个进程\(工作\)的意思。 越小代表系统越闲置，若高于 1 得要注意你的系统进程是否太过繁复了！

  ##### 第二行\(Tasks...\)：显示的是目前进程的总量与个别进程在什么状态\(running, sleeping, stopped, zombie\)。

  比较  
  需要注意的是最后的 zombie 那个数值，如果不是 0 ！好好看看到底是那个 process 变成僵尸了吧？

  ##### 第三行\(%Cpus...\)：显示的是 CPU 的整体负载，每个项目可使用 ? 查阅。

  需要特别注意的是 wa 项目，那个项目代表的是 I/O wait， 通常你的系统会变慢都是 I/O 产生的问题比较大！因此这里得要注意这个项目  
  耗用 CPU 的资源喔！ 另外，如果是多核心的设备，可以按下数字键『1』来切换成不同 CPU 的负载率。

  ##### 第四行与第五行：表示目前的物理内存与虚拟内存 \(Mem/Swap\) 的使用情况。

  要注意的是 swap的使用量要尽量的少！如果 swap 被用的很大量，表示系统的物理内存实在不足！

  ##### 第六行：这个是当在 top 程序当中输入指令时，显示状态的地方。

* #### 下面的部分

  和ps的意思一样

## pstree

```
[root@study ~]# pstree [-A|U] [-up]
```

选项与参数：  
    -A ：各进程树之间的连接以 ASCII 字符来连接；  
    -U ：各进程树之间的连接以万国码的字符来连接。在某些终端接口下可能会有错误；  
    -p ：并同时列出每个 process 的 PID；  
    -u ：并同时列出每个 process 的所属账号名称。  
    范例一：列出目前系统上面所有的进程树的相关性：  
    \[root@study ~\]\# pstree -A  
    systemd-+-ModemManager---2_\[{ModemManager}\] \# 这行是 ModenManager 与其子进程  
     \|-NetworkManager---3_\[{NetworkManager}\] \# 前面有数字，代表子进程的数量！  
    ....\(中间省略\)....  
     \|-sshd---sshd---sshd---bash---bash---sudo---su---bash---pstree &lt;==我们指令执行的相依性  
    ....\(底下省略\)....

    # 注意一下，为了节省版面，所以鸟哥已经删去很多进程了！
    范例二：承上题，同时秀出 PID 与 users
    [root@study ~]# pstree -Aup
    systemd(1)-+-ModemManager(745)-+-{ModemManager}(785)
               |                   `-{ModemManager}(790)
               |-NetworkManager(870)-+-{NetworkManager}(907)
               |                     |-{NetworkManager}(911)
               |                      `-{NetworkManager}(914)
                    ....(底下省略)....
    # 括号内的即是 PID 以及该进程的 owner，如果该进程的拥有者与父进程同就不会列出，但如果与父进程不同，就会列出！



