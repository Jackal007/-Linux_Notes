## 描述 {#描述}

Airolib-ng是aircrack-ng套装里的又一个工具，用来存储和处理网络名和密码表。通过计算PMK，用于WPA/WPA2的破解。Airolib-ng使用了轻量级的SQLite3数据库作为存储机制，在大多数平台上都适用。选用SQLite3综合了可用性、处理能力、存储和磁盘开销等多方面的因素。

破解WPA/WPA2涉及到计算PMK（pairwise master key），而PTK（private transient key）就是从PMK计算得到。通过PTK我们可以计算得到给定包的MIC（message identity code），然后可以判断是否与包里的MIC相同。因此，只要PTK是正确的，PMK就是正确的。

由于使用了pbkdf2算法，导致PMK的计算是非常慢的。然而，对于给定的网络名和密码，PMK是不变的。这就提醒我们可以通过预先计算这些组合（获得PMK），来加速WPA/WPA2的破解。经试验，在aircrack-ng中使用预先计算PMK的方法，可以使破解速度达到&gt;50 000keys/s。

有时计算PMK是必要的，我们可以：

* 预先计算PMK供以后或共享使用
* 使用分布式机器来计算PMK然后再在别的地方使用

## 用法 {#用法}

```
airolib <database><operation> [options]

```

其中：

* database 表示数据库文件。可以是绝对路径
* operation 指定了我们要对数据库进行的操作
* options 有时会需要（取决于指定的操作）

以下是可用的操作：

* –stats：输出一些和数据库相关的信息
* –sql {sql}：执行所给的SQL语句
* –clean \[all\]：清理数据库里一些旧的垃圾。“all”选项会尝试减小文件大小，并做一个完整的检查
* –batch：开始计算所有网络名和密码的组合。此命令需用在aircrack-ng破解之前。当数据库添加了新的SSID或者密码时，也许重新计算
* –verify \[all\]：检验一些随机选择的PMK。如果带有“option”选项，数据库里所有的PMK都要被验证，不正确的会被删除
* –export cowpatty {essid} {file}：导出为cowpatty文件
* –import cowpatty {file}：导入一个cowpatty文件，如果数据库不存在则创建数据库
* import {essid\|passwd} {file}：导入含有网络名或者秘钥列表的文本文件，如果数据库不存在会创建数据库。文件每行需含一个网络名或者密码，以换行符结束（这样读入时就会被当做“enter”）。

## 用法范例 {#用法范例}

### 状态操作 {#状态操作}

输入：

```
airolib-ng testdb --stats

```

其中：

* testdb表示将被创建的数据库
* –stats表示将被执行的操作

屏幕会显示：

```
statsThere are 2 ESSIDs and 232 passwords in the database. 464 out of 464 possible combinations have been computed (100%).

 ESSID   Priority        Done
 Harkonen        64      100.0
 teddy   64      100.0
```

### SQL操作 {#sql操作}

下面的命令会将“VeryImportantESSID”这个SSID赋予最大优先级。输入：

```
airolib-ng testdb --sql 'update essid set prio=(select min(prio)-1 from essid) where essid="VeryImportantESSID";'
```

屏幕显示：

```
update essid set prio=(select min(prio)-1 from essid) where essid="VeryImportantESSID";
 Query done. 1 rows affected.
```

下面的命令将会查找特定样式的PMK

```
airolib-ng testdb --sql 'select hex(pmk) from pmk where hex(pmk) like "%DEADBEEF%"'
```

屏幕会显示：

```
hex(pmk) BF3F122D3CE9ED6C6E7E1D7D13505E0A41EC4C5A3DEADBEEFFEFF597387AFCE3
```

### 清除操作 {#清除操作}

如果是基本的清除，输入：

```
airolib-ng testdb --clean
```

屏幕会显示：

```
cleanDeleting invalid ESSIDs and passwords...
 Deleting unreferenced PMKs...
 Analysing index structure...
 Done.
```

同时尽可能减小文件大小并做完整的检查，输入：

```
airolib-ng testdb --clean all
```

屏幕显示：

```
cleanDeleting invalid ESSIDs and passwords...
 Deleting unreferenced PMKs...
 Analysing index structure...
 Vacuum-cleaning the database. This could take a while...
 Checking database integrity...
 integrity_check
 ok
 Query done. 2 rows affected.
 Done.
```

### 批处理操作 {#批处理操作}

输入：

```
 airolib-ng testdb --batch
```

屏幕会显示：

```
Computed 464 PMK in 10 seconds (46 PMK/s, 0 in buffer). No free ESSID found. Will try determining new ESSID in 5 minutes...
```

### 检验操作 {#检验操作}

检验1000个随机的PMK，输入：

```
 airolib-ng testdb --verify
```

屏幕显示：

```
 airolib-ng testdb --verify
```

检验所有的PMK，输入：

```
 airolib-ng testdb --verify all
```

屏幕显示：

```
 verifyChecking all PMKs. This could take a while...
 ESSID   PASSWORD        PMK_DB  CORRECT
```

### cowpatty表导出操作 {#cowpatty表导出操作}

输入：

```
 airolib-ng testdb --export cowpatty test cowexportoftest
```

屏幕显示：

```
 exportExporting...
 Done.
```

### 导入操作 {#导入操作}

**SSID**

导入ascii字符组成的SSID列表文件，并创建数据库（如果不存在），可输入命令：

```
 airolib-ng testdb --import essid ssidlist.txt
```

其中：

* testdb 表示将被更新的数据库名称，如果不存在则会被创建
* –import 表示将被执行的操作
* essid 后跟SSID列表文件
* ssidlist.txt 表示含有SSID的列表文件，每行一个

屏幕会显示：

```
 importReading...
 Writing...
 Done.
```

**Passwords**

导入ascii字符组成的密码字典文件，并创建数据库（如果不存在），可输入命令：

```
airolib-ng testdb --import passwd password.lst
```

其中：

* testdb 表示将被更新的数据库名称，如果不存在则会被创建
* –import 表示将被执行的操作
* password 后跟密码字典文件
* password.list 表示含有密码字典的文件，每行一个

屏幕会显示：

```
importReading...
 Writing... read, 1814 invalid lines ignored.
 Done.
```

**Cowpatty表**

导入一个Cowpatty表，并创建数据库（如果不存在）。输入命令：

```
airolib-ng testdb --import cowpatty  cowexportoftest
```

其中：

* testdb 表示将被更新的数据库名称，如果不存在则会被创建
* –import 表示将被执行的操作
* cowpatty 后跟cowpatty表文件
* cowexportoftest 文件名，每行一个

## 结合Aircrack-ng使用范例 {#结合aircrack-ng使用范例}

我们最终的目标是加速WPA/WPA2的破解速度。可以使用带参数”-r“的airolib-ng命令来指定数据库，从而使用预先计算的PMK破解秘钥。输入：

```
 aircrack-ng  -r testdb  wpa2.eapol.cap
```

其中：

* -r 指定使用包含PMK的数据库
* testdb 表示数据库文件名
* wpa2.eapol.cap 表示捕捉的含有WPA/WPA2握手包的文件

注：其他任何针对WPA/WPA2的命令选项都可以使用，以上只是一个很有局限性的例子。

## 用法指导 {#用法指导}

### 创建我们的数据库 {#创建我们的数据库}

使用到（方便起见，可使用KALI-Linux）：

* sqlite3（推荐使用最新版本）
* aircrack-ng（推荐使用最新版本）

插入一个essid：

```
echo Harkonen | airolib-ng testdb --import essid -
```

显示：

```
Database <testdb> does not already exist, creating it...
 Database <testdb> sucessfully created
 Reading file...
 Writing...
 Done.
```

插入一个密码，输入命令：

```
echo 12345678 | airolib-ng testdb --import passwd -
```

显示：

```
Reading file...
 Writing...
 Done.
```

开启批处理进程，等待完成。

```
airolib-ng testdb --batch
```

显示：

```
Computed 1 PMK in 0 seconds (1 PMK/s, 0 in buffer). All ESSID processed.
```

检验数据库，确认所有组合都已经被计算：

```
airolib-ng testdb --stats
```

显示：

```
There are 1 ESSIDs and 1 passwords in the database. 1 out of 1 possible combinations have been computed (100%).

 ESSID   Priority        Done
 Harkonen        64      100.0
```

破解WPA/WPA2握手包：

```


aircrack-ng -r testdb -e Harkonen wpa2.eapol.cap
```

显示：

```
 KEY FOUND! [ 12345678 ]
```

### 使用预先建好的数据库 {#使用预先建好的数据库}

另一种测试的方法是，我们可以先下载一个数据库[passphrases.db](http://download.aircrack-ng.org/wiki-files/other/passphrases.db)。aircrack-ng文件中也由此数据库。然后，用这个数据库和aircrack-ng test文件夹里的WPA/WPA2包文件进行测试。包文件的名字为“wpa.cap” 和“wpa2.eapol.cap“。

可输入命令：

```
 aircrack-ng -r passphrases.db wpa.cap
```

或者：

```
 aircrack-ng -r passphrases.db wpa2.eapol.cap
```

## 常见问题及解决方法 {#常见问题及解决方法}

### 编译Airolib-ng {#编译airolib-ng}

默认情况下airolib-ng是没有编译的，可使用如下的命令来编译：

```
make sqlite=true
```

```
make sqlite=true install
```

### 编译错误 {#编译错误}

常是由于SQLite版本过低导致，需要&gt;3.3.13版本的SQLite。出现的错误提示类似于：

    gcc -g -W -Wall -Werror -O3 -D_FILE_OFFSET_BITS=64 -D_REVISION=`../evalrev` -I/usr/local/include -Iinclude -DHAVE_SQLITE   -c -o airolib-ng.o airolib-ng.c
    airolib-ng.c: In function `sql_prepare':
    airolib-ng.c:129: warning: implicit declaration of function `sqlite3_prepare_v2'
    make[1]: *** [airolib-ng.o] Error 1
    make[1]: Leaving directory `/root/1.0-dev/src'
    make: *** [all] Error 2

### 何时需要SQLite补丁 {#何时需要sqlite补丁}

aircrack-ng里包含的SQLite补丁只有在windows环境中才需要。linux安装环境下并不需要。

### Airolib-bg不能打开或者创建数据库 {#airolib-bg不能打开或者创建数据库}

只有在windows环境下，由于airolib-ng的工作路径中含有’ç’, ‘é’, ‘è’, ‘à’等特殊字符，才会导致这个问题。（路径含有空格并不会影响）。解决方法是将airolib-ng移到没有此问题的路径中。

### 错误信息：”invalid lines ignored” {#错误信息invalid-lines-ignored}

这种错误信息一般会在导入密码或者网络名文件中出现。是由于无效的密码或者网络名长度造成的，有效长度为：

* 密码长度8~63字符
* 网络名1~32字符

### 错误信息：”Quitting aircrack-ng…” {#错误信息quitting-aircrack-ng}

如果在随后的aircrack-ng破解中得到此提示信息，说明数据库中缺失网络名（ESSID）。需要添加进去然后重新运行批处理进程。

