> 不需 要透过直接执行该 scripts 就可以来判断是否有问题

```
[dmtsai@study ~]$ sh [-nvx] scripts.sh
选项与参数：
-n ：不要执行 script，仅查询语法的问题；
-v ：再执行 sccript 前，先将 scripts 的内容输出到屏幕上；
-x ：将使用到的 script 内容显示到屏幕上，这是很有用的参数！
范例一：测试 dir_perm.sh 有无语法的问题？
[dmtsai@study ~]$ sh -n dir_perm.sh
# 若语法没有问题，则不会显示任何信息！
范例二：将 show_animal.sh 的执行过程全部列出来～
[dmtsai@study ~]$ sh -x show_animal.sh
+ PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:/root/bin
+ export PATH
+ for animal in dog cat elephant
+ echo 'There are dogs.... '
There are dogs....
+ for animal in dog cat elephant
+ echo 'There are cats.... '
There are cats....
+ for animal in dog cat elephant
+ echo 'There are elephants.... '
There are elephants....
```



