## 打包再压缩

虽然 gzip, bzip2, xz 也能够针对目录来进行压缩，不过， 这两个指令对目录的压缩指的是『将目录内的所有文件 "分别" 进行压缩』的动作！ 而不像在 Windows 的系统，可以使用类似 WinRAR 这一类的压缩软件来将好多数据『包成一个文 件』的样式。 这种将多个文件或目录包成一个大文件的指令功能，我们可以称呼他是一种『打包指令』！那 Linux 有没有这种打包指令呢？是有的！那就是鼎鼎大名的 tar 这个玩意儿了！ tar 可以将多个目录或文 件打包成一个大文件，同时还可以透过 gzip/bzip2/xz 的支持，将该文件同时进行压缩！更有趣的是， 由于 tar 的使用太广泛了，目前 Windows 的 WinRAR 也支持 .tar.gz 档名的解压缩。

#### 打包

将多个文件集结成一个文件

> 打包后的文件叫做：tarfile；打包压缩后的文件叫做：tarball

* -c: 建立压缩档案
* -x：解压
* -t：查看内容
* -r：向压缩归档文件末尾追加文件
* -u：更新原压缩包中的文件

这五个是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但只能用其中一个。

下面的参数是根据需要在压缩或解压档案时可选的。

* -z：有gzip属性的
* -j：有bz2属性的
* -Z：有compress属性的
* -v：显示所有过程
* -O：将文件解开到标准输出

参数-f是必须的

* -f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名。
* -p\(小写\) ：保留备份数据的原本权限与属性，常用于备份\(-c\)重要的配置文件
* -P\(大写\) ：保留绝对路径，亦即允许备份数据中含有根目录存在，那么解压的时候就会自动回到原来的绝对路径下
* --exclude=FILE：在压缩的过程中，不要将 FILE 打包！

例子

```
\# tar -cf all.tar \*.jpg 这条命令是将所有.jpg的文件打成一个名为all.tar的包。-c是表示产生新的包，-f指定包的文件名。

\# tar -rf all.tar \*.gif 这条命令是将所有.gif的文件增加到all.tar的包里面去。-r是表示增加文件的意思。

\# tar -uf all.tar logo.gif 这条命令是更新原来tar包all.tar中logo.gif文件，-u是表示更新文件的意思。

\# tar -tf all.tar 这条命令是列出all.tar包中所有文件，-t是列出文件的意思,在不解压的情况下查看压缩包的内容

\# tar -xf all.tar 这条命令是解出all.tar包中所有文件，-x是解开的意思
```

压缩

```
tar –cvf jpg.tar \*.jpg \#将目录里所有jpg文件打包成tar.jpg

tar –czf jpg.tar.gz \*.jpg \#将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz

tar –cjf jpg.tar.bz2 \*.jpg \#将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2

tar –cZf jpg.tar.Z \*.jpg \#将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z
```

解压

```
tar –xvf file.tar    \#解压 tar包

tar -xzvf file.tar.gz    \#解压tar.gz

tar -xjvf file.tar.bz2    \#解压 tar.bz2tar –xZvf file.tar.Z //解压tar.Z

tar -xvf file.tar -C DestinationDirectory    \#将文件解压到DestinationDirectory中

tar -xv -f xxx.tar target    \#将压缩包中的某个文件解压出来
```

总结

|  |  |
| :--- | :--- |
| \*.tar | 用 tar –xvf 解压 |
| \*.gz | 用 gzip -d或者gunzip 解压 |
| \*.tar.gz和\*.tgz | 用 tar –xzf 解压 |
| \*.bz2 | 用 bzip2 -d或者用bunzip2 解压 |
| \*.tar.bz2 | 用tar –xjf 解压 |
| \*.Z | 用 uncompress 解压 |
| \*.tar.Z | 用tar –xZf 解压 |



