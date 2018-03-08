# **crunch简介**

 crunch是一款运行在linux中的字典生成工具，可以灵活的定制自己的密码字典文件。

**crunch使用语法及参数**

```
 crunch [min-len  max-len  charset string ] [options]
```

> 参数解释
>
> **min-len** 开始的最小长度字符串（这个选项是必须的）  
> **max-len** 结束的最大长度字符串（这个选项是必须的）  
> **charset string** 要生成密码包含的字符集（小写字符、大写字符、数字、符号），这个选项是可选的，如果你不写这个选项，将使用默认字符集（默认为小写字符）。如果你想生成的密码包含空格字符，你可以使用来代替，  
>  如：123abc  
>  你可以把带有空格的字符集放在双引号“”中，如“123abc ”

  
**OPTIONS**

* **-b** 按指定的大小单位分割字典文件成若干个指定的大小的字典，避免一个字典文件过大，如：

```
./crunch 4 5 -b 20mib -o START 按每个文件20mib分割字典文件。
 说明：单位有 kb, mb, gb, kib, mib, gib ，前三个单位是以1000单位计算的，后三个是以1024计算的。
 注意数字和单位之间没有空格，例如：500mb 是正确的， 500 mb 格式是不对的（中间有空格），-b参数必须和-o START结合使用
```

* **-c** 指定要写入字典文件中的行数，只在有 -o START参数时才生效，单个字典超过指定的行数会被分割成两个或多个字典文件。例如： 

```
-c 5000
```

* **-d** 限制同一字符连续出现的数量， -d 2 表示限制一个字符最多连续出现2次如 aabd，ccda。aaab就表示超过了2个字符连续的限制了。
* **-e** 定义到那个字符串就停止生成密码，例如： -e 999999 就表示在生成密码到99999时就停止生成密码
* **-f** 调用密码库文件，例如：/usr/share/crunch/charset.lst
* **-i** 改变输出格式，例如： 原本输入为aaa,aab,aac,aad再使用了-i之后，就会变成aaa,baa,caa,daa的格式了
* **-l** When you use the -t option this option tells crunch which symbols should be treated as literals. This will allow you to use the placeholders as letters in the pattern. The -l option should be the same length as the -t option. See example 15.
* **-m** 与-p搭配使用
* **-o** 输出生成的密码到指定的文件，如： -o lybbnwordlist.txt
* **-p** 定义密码元素
* **-q** 读取密码元素
* **-r** 定义从某一个地方重新开始
* **-s** 第一个密码，从自己定义的密码xxx开始
* **-t** 定义密码输出格式 1.@代表插入小写字母  2.，代表插入大写字母  3. %代表插入数字  4.^代表插入符号
* **-u** The -u option disables the printpercentage thread. This should be the last option.
* **-z** 压缩生成的字典文件，有效的参数是gzip, bzip2, lzma, and 7z，其中gzip压缩最快，bzip2压缩速度比gzip慢单压缩效率比gzip好，7z压缩速度最慢，但是压缩效率最高。



