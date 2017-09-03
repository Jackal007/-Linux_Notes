```
[root@study ~]# head -n 4 /etc/gshadow
root:::
bin:::
daemon:::
sys:::
```

这四个字段的意义为：

1. 组名
2. 密码栏，同样的，开头为 ! 表示无合法密码，所以无群组管理员
3. 群组管理员的账号 \(相关信息在 gpasswd 中介绍\)
4. 有加入该群组支持的所属账号 \(与 /etc/group 内容相同！\)





