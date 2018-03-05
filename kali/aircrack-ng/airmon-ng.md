# Airmon-ng {#airmon-ng}

### Description

 This script can be used to enable monitor mode on wireless interfaces. It may also be used to go back from monitor mode to managed mode. Entering the airmon-ng command without parameters will show the interfaces status.

### Usage

usage: airmon-ng &lt;start\|stop\|check&gt; &lt;interface&gt; \[channel or frequency\]

 Where:

* &lt;start\|stop&gt;
   indicates if you wish to start or stop the interface. \(Mandatory\)
* &lt;interface&gt;
   specifies the interface. \(Mandatory\) 
* \[channel\] optionally set the card to a specific channel.

### 例子

```
~# airmon-ng
PHY	Interface	Driver		Chipset

phy0	wlan0		ath9k_htc	Atheros Communications, Inc. AR9271 802.11n
~# airmon-ng start wlan0
~# airmon-ng stop wlan0mon
```



