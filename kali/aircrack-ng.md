![](https://www.aircrack-ng.org/resources/aircrack-ng-new-logo.jpg "这里写图片描述")

[from:http://blog.csdn.net/tonyzhejiang/article/details/72152512](http://blog.csdn.net/tonyzhejiang/article/details/72152512)

**Aircrack- NG**是一个完整的工具来评估Wi-Fi网络安全套件_**\(ex.1\)**_。

它专注于WiFi安全的不同领域：

**监控**：数据包捕获和导出数据到文本文件，以供第三方工具进一步处理。  
**攻击**：通过数据包注入回放攻击，去认证，伪造接入点等。  
**测试**：检查WiFi卡和驱动程序的能力（捕捉和注入）。  
**破解**：**WEP** 和 **WPA PSK**（**WPA 1和2**）。  
 所有的工具都是命令行，它允许重的脚本。很多GUI都利用了这个功能。它主要工作在 Linux，但也支持 Windows，OS X，FreeBSD，OpenBSD，NetBSD以及Solaris甚至eComStation 2。

---

* **使用airmon-ng查看你无线网卡。**  
  `sudo airmon-ng`  
  找到interface栏，记下你的无线网卡

* **使用airmon-ng开启网卡监听模式。**  
  记下你的monitor名称  
  `sudo airmon-ng start wlan0`





