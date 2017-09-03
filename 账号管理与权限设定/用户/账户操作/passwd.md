### passwd：

```
[root@study ~]# passwd [--stdin] [账号名称] <==所有人均可使用来改自己的密码
[root@study ~]# passwd [-l] [-u] [--stdin] [-S] [-n 日数] [-x 日数] [-w 日数] [-i 日期] 账号 <==root 功能
```

> 选项与参数：
>
> --stdin ：可以透过来自前一个管线的数据，作为密码输入，对 shell script 有帮助！
>
> **-l ：是 Lock 的意思，会将 /etc/shadow 第二栏最前面加上 ! 使密码失效；**
>
> -u ：与 -l 相对，是 Unlock 的意思！
>
> -S ：列出密码相关参数，亦即 shadow 文件内的大部分信息。
>
> -n ：后面接天数，shadow 的第 4 字段，多久不可修改密码天数
>
> -x ：后面接天数，shadow 的第 5 字段，多久内必须要更动密码
>
> -w ：后面接天数，shadow 的第 6 字段，密码过期前的警告天数
>
> -i ：后面接『日期』，shadow 的第 7 字段，密码失效日期

---

###  chage

```
[root@study ~]# chage [-ldEImMW] 账号名
```

> 选项与参数：
>
> -l ：列出该账号的详细密码参数；
>
> -d ：后面接日期，修改 shadow 第三字段\(最近一次更改密码的日期\)，格式 YYYY-MM-DD
>
> -E ：后面接日期，修改 shadow 第八字段\(账号失效日\)，格式 YYYY-MM-DD
>
> -I ：后面接天数，修改 shadow 第七字段\(密码失效日期\)
>
> -m ：后面接天数，修改 shadow 第四字段\(密码最短保留天数\)
>
> -M ：后面接天数，修改 shadow 第五字段\(密码多久需要进行变更\)
>
> -W ：后面接天数，修改 shadow 第六字段\(密码过期前警告日期\)
>
> ```
> 范例二：建立一个名为 agetest 的账号，该账号第一次登入后使用默认密码，但必须要更改过密码后，
>  使用新密码才能够登入系统使用 bash 环境
> [root@study ~]# useradd agetest
> [root@study ~]# echo "agetest" | passwd --stdin agetest
> [root@study ~]# chage -d 0 agetest
> [root@study ~]# chage -l agetest | head -n 3
> Last password change : password must be changed
> Password expires : password must be changed
> Password inactive : password must be changed
> # 此时此账号的密码建立时间会被改为 1970/1/1 ，所以会有问题！
> ```



