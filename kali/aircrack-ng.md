![](https://www.aircrack-ng.org/resources/aircrack-ng-new-logo.jpg "这里写图片描述")

[http://netsecurity.51cto.com/art/201105/264844\_all.htm](http://netsecurity.51cto.com/art/201105/264844_all.htm)

Aircrack-ng是一款用于破解无线802.11WEP及WPA-PSK加密的工具，该工具在2005年11月之前名字是Aircrack，在其2.41版本之后才改名为Aircrack-ng。

Aircrack-ng主要使用了两种攻击方式进行WEP破解：一种是FMS攻击，该攻击方式是以发现该WEP漏洞的研究人员名字（Scott Fluhrer、Itsik Mantin及Adi Shamir）所命名；另一种是KoreK攻击，经统计，该攻击方式的攻击效率要远高于FMS攻击。当然，最新的版本又集成了更多种类型的攻击方式。对于无线黑客而言，Aircrack-ng是一款必不可缺的无线攻击工具，可以说很大一部分无线攻击都依赖于它来完成；而对于无线安全人员而言，Aircrack-ng也是一款必备的无线安全检测工具，它可以帮助管理员进行无线网络密码的脆弱性检查及了解无线网络信号的分布情况，非常适合对企业进行无线安全审计时使用。

它专注于WiFi安全的不同领域：

**监控**：数据包捕获和导出数据到文本文件，以供第三方工具进一步处理。  
**攻击**：通过数据包注入回放攻击，去认证，伪造接入点等。  
**测试**：检查WiFi卡和驱动程序的能力（捕捉和注入）。  
**破解**：**WEP** 和 **WPA PSK**（**WPA 1和2**）。  
 所有的工具都是命令行，它允许重的脚本。很多GUI都利用了这个功能。它主要工作在 Linux，但也支持 Windows，OS X，FreeBSD，OpenBSD，NetBSD以及Solaris甚至eComStation 2。

Aircrack-ng包含了多款工具的无线攻击审计套装，具体见下表为Aircrack-ng包含的组件具体列表。

| **组件名称** | **描述** |
| :--- | :--- |
| **aircrack-ng** | 主要用于WEP及WPA-PSK密码的恢复，只要airodump-ng收集到足够数量的数据包，aircrack-ng就可以自动检测数据包并判断是否可以破解 |
| **airmon-ng** | 用于改变无线网卡工作模式，以便其他工具的顺利使用 |
| **airodump-ng** | 用于捕获802.11数据报文，以便于aircrack-ng破解 |
| **aireplay-ng** | 在进行WEP及WPA-PSK密码恢复时，可以根据需要创建特殊的无线网络数据报文及流量 |
| **airserv-ng** | 可以将无线网卡连接至某一特定端口，为攻击时灵活调用做准备 |
| **airolib-ng** | 进行WPA Rainbow Table攻击时使用，用于建立特定数据库文件 |
| **airdecap-ng** | 用于解开处于加密状态的数据包 |
| **tools** | 其他用于辅助的工具，如airdriver-ng、packetforge-ng等 |

---

```
Common options:

      -a <amode> : force attack mode (1/WEP, 2/WPA-PSK)
      -e <essid> : target selection: network identifier[应该是局域网的网关mac地址]
      -b <bssid> : target selection: access point's MAC[主机mac地址]
      -p <nbcpu> : # of CPU to use  (default: all CPUs)
      -q         : enable quiet mode (no status output)
      -C <macs>  : merge the given APs to a virtual one
      -l <file>  : write key to file

  Static WEP cracking options:

      -c         : search alpha-numeric characters only
      -t         : search binary coded decimal chr only
      -h         : search the numeric key for Fritz!BOX
      -d <mask>  : use masking of the key (A1:XX:CF:YY)
      -m <maddr> : MAC address to filter usable packets
      -n <nbits> : WEP key length :  64/128/152/256/512
      -i <index> : WEP key index (1 to 4), default: any
      -f <fudge> : bruteforce fudge factor,  default: 2
      -k <korek> : disable one attack method  (1 to 17)
      -x or -x0  : disable bruteforce for last keybytes
      -x1        : last keybyte bruteforcing  (default)
      -x2        : enable last  2 keybytes bruteforcing
      -X         : disable  bruteforce   multithreading
      -y         : experimental  single bruteforce mode
      -K         : use only old KoreK attacks (pre-PTW)
      -s         : show the key in ASCII while cracking
      -M <num>   : specify maximum number of IVs to use
      -D         : WEP decloak, skips broken keystreams
      -P <num>   : PTW debug:  1: disable Klein, 2: PTW
      -1         : run only 1 try to crack key with PTW

  WEP and WPA-PSK cracking options:

      -w <words> : path to wordlist(s) filename(s)

  WPA-PSK options:

      -E <file>  : create EWSA Project file v3
      -J <file>  : create Hashcat Capture file
      -S         : WPA cracking speed test
      -r <DB>    : path to airolib-ng database
                   (Cannot be used with -w)

  Other options:

      -u         : Displays # of CPUs & MMX/SSE support
      --help     : Displays this usage screen
```

---

* **使用airmon-ng查看你无线网卡。**  
  `sudo airmon-ng`  
  找到interface栏，记下你的无线网卡

* **使用airmon-ng开启网卡监听模式。**  
  记下你的monitor名称  
  `sudo airmon-ng start wlan0`

---

### 使用字典爆破握手包

这一步是整个密码破解的关键，能否成功，取决与你的CPU是否强大，还有你字典的大小，以及字典的质量，密码的复杂程度。

```
aircrack-ng sofia-01.cap -w /root/sofia/wifi-hack/dic/wpa专用.txt
```

解释：“sofia-01.cap”为指定我们需要破解的握手包，直接写文件名表示文件在当前文件夹下，"-w"指定字典文件后面跟着字典的路径。

