1. 配置wpa_supplicant.conf文件：

    `
    wpa_passphrase ssid(wifi name) psk(wifi pass)
    `
    
2. 启动wpa_supplicant后台服务程序：

    `/sbin/wpa_supplicant -d -iwlan0 -c /etc/wpa_supplicant.conf
    `

    > 其中：
    >
    > -d ：增加调试信息
    >
    > -Dwext ：wext，驱动名称
    >
    > -iwln0 ：wlan0，网络接口名称
    >
    > /etc/wpa_supplicant.conf ：配置文件

network里面的内容就是接入AP的配置，wpa_supplicant启动时会以该配置去连接wifi。几个选项的介绍如下，更多的解释可以参考官方的配置文件注释。
o	ssid 接入点名称
o	scan_ssid=1 如果你的无线接入点是隐藏的，那么这个就是必须的。
o	psk=xx 是加密后的密码，用wpa_passphrase自动生成的
o	proto=RSN WPA2只是RSN的一个别名，支持WPA和WPA2
关于psk加密的生成，使用wpa_passphrase命令如下：
$ wpa_passphrase TPLINK 12345678
 network={  
     ssid="TPLINK"
     #psk="12345678"
     psk=992194d7a6158009bfa25773108291642f28a0c32a31ab2556a15dee97ef0dbb
 } 
这里表示名为TPLINK的接入点，密码是12345678，输出就是该接入点在wpa_supplicant.conf里面的配置内容。
执行wpa_cli工具进行搜索和连接
 wpa_cli -i wlan0
wlan0是wifi接口名称，以上名列会进入交互模式，然后进行具体的动作。具体支持的命令可以通过help命令来查看，以下进介绍常用的几个命令。
o	scan 扫描当前可以的WiFi列表
o	scan_result 查看上次scan的扫描结果
o	add_network 添加一个AP连接网络
o	set_network 设置连接网络的相关参数
o	get_network 获取连接网络的参数
o	remove_network 删除一个连接网络
o	enable_network 连接到指定的AP
o	disable_network 禁止一个网络
o	disconnet 端口当前的AP连接
o	status 查看当前的连接状态信息
o	save_config 保存配置
一些交互连接的示例：
o	连接无加密的AP
o	  add_network
o	  set_network 0 ssid "ap1"
o	  set_network 0 key_mgmt NONE
o	  enable_network 0
o	  quit
o	连接WEP加密AP
o	  add_network
o	  set_network 1 ssid "ap2"
o	  set_network 1 key_mgmt NONE
o	  set_network 1 wep_key0 "your ap password"
o	  enable_network 1
o	连接WPA-PSK/WPA2-PSK加密的AP
o	  add_network
o	  set_network 2 ssid "ap3"
o	  set_network 2 psk "your pre-shared key"
  enable_network 2


https://my.oschina.net/shelllife/blog/1532270

使用管理员权限（经由sudo），编辑/etc/apt/sources.list文件。参考命令行为：

$ sudo nano /etc/apt/sources.list
1
用#注释掉原文件内容，用以下内容取代：

deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ stretch main contrib non-free rpi
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ stretch main contrib non-free rpi
1
2
使用管理员权限（经由sudo），编辑/etc/apt/sources.list.d/raspi.list文件。参考命令行为：

$ sudo nano /etc/apt/sources.list.d/raspi.list
1
用#注释掉原文件内容，用以下内容取代：

deb http://mirror.tuna.tsinghua.edu.cn/raspberrypi/ stretch main ui
deb-src http://mirror.tuna.tsinghua.edu.cn/raspberrypi/ stretch main ui
1
2
注意： 网址末尾的raspbian重复两次是必须的。因为Raspbian的仓库中除了APT软件源还包含其他代码。APT软件源不在仓库的根目录，而在raspbian/子目录下。

编辑镜像站后，请使用sudo apt-get update命令，更新软件源列表，同时检查您的编辑是否正确。

使用HTTPS可以有效避免国内运营商的缓存劫持，但需要事先安装apt-transport-https

--------------------- 
作者：v投笔从容v 
来源：CSDN 
原文：https://blog.csdn.net/la9998372/article/details/77886806 
版权声明：本文为博主原创文章，转载请附上博文链接！
