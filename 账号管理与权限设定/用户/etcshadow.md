### /etc/shadow

我们知道很多程序的运作都与权限有关，而权限与 UID/GID 有关！因此各程序当然需要读取 /etc/passwd 来了解不同账号的权限。 因此 /etc/passwd 的权限需设定为 -rw-r--r-- 这样的情况， 虽 然早期的密码也有加密过，但却放置到 /etc/passwd 的第二个字段上！这样一来很容易被有心人士所 窃取的， 加密过的密码也能够透过暴力破解法去 trial and error \(试误\) 找出来！ 因为这样的关系，所以后来发展出将密码移动到 /etc/shadow 这个文件分隔开来的技术， 而且还加 入很多的密码限制参数在 /etc/shadow 里头:

```bash
[root@study ~]# head -n 4 /etc/shadow
root:$6$wtbCCce/PxMeE5wm$KE2IfSJr.YLP7Rcai6oa/T7KFhO...:16559:0:99999:7::: <==底下说明用
bin:*:16372:0:99999:7:::
daemon:*:16372:0:99999:7:::
adm:*:16372:0:99999:7:::
```

基本上， shadow 同样以『:』作为分隔符，如果数一数，会发现共有九个字段啊，这九个字段的用途是这样的：

##### 账号名称：

由于密码也需要与账号对应啊～因此，这个文件的第一栏就是账号，必须要与 /etc/passwd 相同才行！

##### 密码：

这个字段内的数据才是真正的密码，而且是经过编码的密码 \(加密\) 啦！ 你只会看到有一些特殊符号的字母就是了！需要特别留意的是，虽然这些加密过的密码很难被解出来， 但是『很难』不等于『不会』，所以，这个文件的预设权限是『-rw-------』或者是『----------』，亦即只有 root 才可以读写就是了！你得随时注意，不要不小心更动了这个文件的权限呢！

另外，由于各种密码编码的技术不一样，因此不同的编码系统会造成这个字段的长度不相同。 举例来说，旧式的 DES, MD5 编码系统产生的密码长度就与目前惯用的 SHA 不同\(注 2\)！SHA 的密码长度明显的比较长些。由于固定的编码系统产生的密码长度必须一致，因此『当你让这个字段的长度改变后，该密码就会失效\(算不出来\)』。 很多软件透过这个功能，在此字段前加上 ! 或 \* 改变密码字段长度，就会让密码『暂时失效』了。

##### 最近更动密码的日期：

这个字段记录了『更动密码那一天』的日期，不过，很奇怪呀！在我的例子中怎么会是 16559 呢？呵呵，

这个是因为计算 Linux 日期的时间是以 1970 年 1 月 1 日作为 1 而累加的日期，1971 年 1 月 1 日则

为 366 啦！ 得注意一下这个资料呦！上述的 16559 指的就是 2015-05-04 那一天啦！了解乎？ 而想要了

解该日期可以使用本章后面 chage 指令的帮忙！至于想要知道某个日期的累积日数， 可使用如下的程序计

算：

```bash
[root@study ~]# echo $(($(date --date="2015/05/04" +%s)/86400+1))
16559
```

上述指令中，2015/05/04 为你想要计算的日期，86400 为每一天的秒数， %s 为 1970/01/01 以来的累积总

秒数。 由于 bash 仅支持整数，因此最终需要加上 1 补齐 1970/01/01 当天。

##### 密码不可被更动的天数：\(与第 3 字段相比\)

第四个字段记录了：这个账号的密码在最近一次被更改后需要经过几天才可以再被变更！如果是 0 的话，

表示密码随时可以更动的意思。这的限制是为了怕密码被某些人一改再改而设计的！如果设定为 20 天的

话，那么当你设定了密码之后， 20 天之内都无法改变这个密码呦！

##### 密码需要重新变更的天数：\(与第 3 字段相比\)

经常变更密码是个好习惯！为了强制要求用户变更密码，这个字段可以指定在最近一次更改密码后， 在多

少天数内需要再次的变更密码才行。你必须要在这个天数内重新设定你的密码，否则这个账号的密码将会

『变为过期特性』。 而如果像上面的 99999 \(计算为 273 年\) 的话，那就表示，呵呵，密码的变更没有强

制性之意。

##### 密码需要变更期限前的警告天数：\(与第 5 字段相比\)

当账号的密码有效期限快要到的时候 \(第 5 字段\)，系统会依据这个字段的设定，发出『警告』言论给这个

账号，提醒他『再过 n 天你的密码就要过期了，请尽快重新设定你的密码呦！』，如上面的例子，则是密码

到期之前的 7 天之内，系统会警告该用户。

##### 密码过期后的账号宽限时间\(密码失效日\)：\(与第 5 字段相比\)

密码有效日期为『更新日期\(第 3 字段\)』+『重新变更日期\(第 5 字段\)』，过了该期限后用户依旧没有更新密

码，那该密码就算过期了。 虽然密码过期但是该账号还是可以用来进行其他工作的，包括登入系统取得

bash 。不过如果密码过期了， 那当你登入系统时，系统会强制要求你必须要重新设定密码才能登入继续使

用喔，这就是密码过期特性。

那这个字段的功能是什么呢？是在密码过期几天后，如果使用者还是没有登入更改密码，那么这个账号的

密码将会『失效』， 亦即该账号再也无法使用该密码登入了。要注意密码过期与密码失效并不相同。

##### 账号失效日期：

这个日期跟第三个字段一样，都是使用 1970 年以来的总日数设定。这个字段表示： 这个账号在此字段规

定的日期之后，将无法再使用。 就是所谓的『账号失效』，此时不论你的密码是否有过期，这个『账号』  
都不能再被使用！ 这个字段会被使用通常应该是在『收费服务』的系统中，你可以规定一个日期让该账号

不能再使用啦！

##### 保留：

最后一个字段是保留的，看以后有没有新功能加入

