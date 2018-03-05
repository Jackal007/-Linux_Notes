# Airserv-ng {#airserv-ng}

## Description {#description}

 Airserv-ng is a wireless card server which allows multiple wireless application programs to independently use a wireless card via a client-server TCP network connection. All operating system and wireless card driver specific code is incorporated into the server. This eliminates the need for each wireless application to contain the complex wireless card and driver logic. It is also supports multiple operating systems.

 When the server is started, it listens on a specific IP and TCP port number for client connections. The wireless application then communicates with the server via this IP address and port. When using the aircrack-ng suite functions, you specify “&lt;server IP address&gt; colon &lt;port number&gt;” instead of the network interface. An example being 127.0.0.1:666.

 This allows for a number of interesting possibilities:

*  By eliminating the wireless card/driver complexity, software developers can concentrate on the application functionality. This will lead to a larger set of applications being available. It also dramatically reduces the maintenance effort.
*  Remote sensors are now easy to implement. Only a wireless card and airserv-ng are required to be running on the remote sensor. This means that small embedded systems can easily be created.
*  You can mix and match operating systems. Each piece can run on a different operating system. The server and each of the applications can potentially run under a different operating system.
*  Some wireless cards do not allow multiple applications to access them at once. This constraint is now eliminated with the client-server approach.
*  By using TCP networking, the client and server can literally be in different parts of the world. As long as you have network connectivity, then it will work.

## Usage {#usage}

 Usage: airserv-ng &lt;opts&gt;

 Where:

*  -p &lt;port&gt;
   TCP port to listen on. Defaults to 666.
*  -d &lt;dev&gt;
   wifi device to serve.
*  -c &lt;chan&gt;
   Channel to start on.
*  -v &lt;level&gt;
   debug level

### Debug Levels {#debug_levels}

 There are three debug levels. Debug level 1 is the default if you do not include the “-v” option.

**Debug level of 1**  
 Contents: Shows connect and disconnect messages.

 Examples: Connect from 127.0.0.1 Death from 127.0.0.1

**Debug level of 2**  
 Contents: Shows channel change requests and invalid client command requests in addition to the debug level 1 messages. The channel change request indicates which channel the client requested.

 Examples: \[127.0.0.1\] Got setchan 9 \[127.0.0.1\] handle\_client: net\_get\(\)

**Debug level of 3**  
 Contents: Displays a message each time a packet is sent to the client. The packet length is indicated as well. This level includes both the level 1 and level 2 messages.

 Examples: \[127.0.0.1\] Sending packet 97 \[127.0.0.1\] Sending packet 97

## Usage Examples {#usage_examples}

 In all cases, you must first put your wireless card into monitor mode using [airmon-ng](http://www.aircrack-ng.org/doku.php?id=airmon-ng) or a similar technique.

### Local machine {#local_machine}

 This scenario has all the components running on the same system.

 Start the program with:

```
 airserv-ng -d ath0
```

 Where:

*  -d ath0 is the network card to use. Specify the network interface for your particular card.

 The system responds:

```
 Opening card ath0
 Setting chan 1
 Opening sock port 666
 Serving ath0 chan 1 on port 666
```

 At this point you may use any of the aircrack-ng suite programs and specify “127.0.0.1:666” instead of the network interface. 127.0.0.1 is the “loopback” IP of your PC and 666 is the port number that the server is running on. Remember that 666 is the default port number.

 Example:

```
airodump-ng 127.0.0.1:666
```

 It will start scanning all networks.

### Remote machine {#remote_machine}

 This scenario has the server running on one system with an IP address of 192.168.0.1 and the applications \(airodump-ng, aireplay-ng, …\) on another system.

 Start the program with:

```
 airserv-ng -d ath0
```

 Where:

*  -d ath0 is the network card to use. Specify the network interface for your particular card.

 The system responds:

```
 Opening card ath0
 Setting chan 1
 Opening sock port 666
 Serving ath0 chan 1 on port 666
```

 At this point you may use any of the aircrack-ng suite programs on the second system and specify “192.168.0.1:666” instead of the network interface. 192.168.0.1 is the IP address of the server system and 666 is the port number that the server is running on. Remember that 666 is the default port number.

 On the second system, you would enter “airodump-ng 192.168.0.1:666” to start scanning all the networks. You may run aircrack-ng applications on as many other systems as you want by simply specifying “192.168.0.1:666” as the network interface.

 Example:

```
airodump-ng -c 6 192.168.0.1:666
```

## Usage Tips {#usage_tips}

 None at this time.

## Usage Troubleshooting {#usage_troubleshooting}

 Is your card in monitor mode? Make sure your card is in monitor mode prior to starting airserv-ng.

 Are you connecting to the correct IP and TCP port number? Double check this. Remember that the default port number is 666. You can use the [aireplay-ng injection test](http://www.aircrack-ng.org/doku.php?id=injection_test#airserv-ng_test) to verify connectivity and proper operation.

 Firewall software can block communication so make sure the following allows communication to and from the server port. This applies to both the machine running airserv-ng and the client machine. Items to check:

*  IPTables on linux system.
*  Firewalls software on linux and especially Windows
*  Any firewalls along the TCP network between the client and server

 Some software can also affect successful operation:

*  Anti-Spyware software
*  Anti-Virus software

 To confirm that airserv-ng is listening the expected port:

*  Under linux: “netstat -an” or “lsof -i” and look for the port number.
*  Under Window, open a command line and type “netstat -an” then look for the port number.

 At present, there are known issues with the madwifi-ng drivers for atheros-based cards. Channel hopping and setting the channel does not always work correctly. Very often the card is not set to the requested channel and/or the hopping does not take place.

