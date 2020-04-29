Cisco IOS usage
===============

The Cisco IOS (Internetworking Operating System) runs on every Cisco device, regardless of the type or size.

Accessing the IOS
-----------------

You can access the IOS on your device by multiple ways. These are:

* Console port
	- Using an serial cable, connecting to the Console port and channeling I/O with a serial monitor.
* SSH
	- Every device with an IP can be accessed via SSH. The device has to be configured before one can access via SSH.
* Telnet
	- Unlike SSH, Telnet is not secure. Authentication, passwords and commands are transferred in plain text. Use SSH instead.
	
Primary command modes
---------------------

There are two command modes

Command Mode 		  | User Exec Mode | Privileged Exec Mode |
--------------------- | -------------- | -------------------- |
Description  		  | Only limited access | Allows all commands and modes |
Default Device Prompt | Switch> | Switch# |

Switching between these modes is done via the command: `enable` or `disable`, or if you are in a hurry, just `en`.


Configuration Command Modes
---------------------------

To configure a device, the user must enter the global configuration mode. Global conf mode is identified by the `(config)#` at the prompt.  
Example: `Switch(config)#`. To return to Privileged Exec Mode enter `end` or use `CTRL + Z` shortcut.

The user can switch to different subconfiguration modes. These include:
* Line Configuration Mode
	- Used to configure console, SSH or AUX access
	- Identified by `Switch(config-line)#`
	- Switch to a specific line via `line console 0` or `line vty 0 15`
* Interface Configuration Mode
	- Used to configure an switch port or router network interface
	- Identified by `Switch(config-if)`

Switching between these is done via: 
* Line configuration mode:
	- `configure terminal` or `conf t` if you are in a hassle
* Interface configuration mode:
	- `interface vlan1`, or `interface fastethernet0/1`

Change Hostname
---------------
```
Switch> en
Switch# conf t
Switch# hostname Switch-Floor-1
Switch-Floor-1#
```

Secure Device Access
--------------------

There are multiple ways to secure your device from unauthorized access. You can restrict access of the privileged mode, as well as the various ways to connect to the device.
Always remember, passwords are stored in plain text. Always make sure to encrypt these files.
__Privileged Exec Mode Example__

```
Router> enable
Router#
Router# conf terminal
Router(config)# enable secret ultrasecretpassword
Router(config)# exit
Router#
```

__Restrict Console Access__

```
Router> enable
Router#
Router# conf terminal
Router(config)# line console 0
Router(config-line)# password ultrasecretpassword
Router(config-line)# login
Router(config-line)# exit
Router(config)#
```

__Restrict SSH / TTY Access__

```
Router> enable
Router#
Router# conf terminal
Router(config)# line vty 0 15
Router(config-line)# password ultrasecretpassword
Router(config-line)# login
Router(config-line)#
```

__Encrypt Passwords__

```
Router> enable
Router#
Router# conf terminal
Router(config)# service password-encryption
Router(config)# exit
```


Change Banner / Message of the Day / MotD
-------------------

```
Router> enable
Router#
Router# conf terminal
Router(config)# banner motd "Pls dont hack me!"
```

Viewing and Saving Running Configuration
--------------------------

__View__
```
Router> enable
Router#
Router# show running-config
```
```
Router> enable
Router#
Router# show startup-config
```

__Save__
```
Router> enable
Router#
Router# copy running-config startup-config
```

If, for whatever reason, you end up with an undesired config state, you can always use the `reload` command. But be aware that this command shuts the device off briefly, disabling its network capabilities.
The same is the case with the `erase startup-config` command. But the erase command will delete every configuration you did, like passwords, hostnames or motd's.

In case you f'd up but havent saved yet, you can overwrite the existing config with the startup config. Just run:
```
Router> enable
Router#
Router# copy startup-config running-config
```


Addressing Schemes
==================

So you want to check the IP address of your computer in your network?

Windows: `ipconfig`
Linux: `ip a`


Switch Virtual Interface (SVI) Configuration
-----------------------------

A Switch does not need an IP address to work properly. It still comes in handy to assign an IP address to the Switch for remote configuration.
This is how to do it:

```
Switch> en
Switch# conf t
Switch(config)# intercace vlan 1
Switch(config-if)# ip add 192.168.51.104 255.255.255.0
Switch(config-if)# no shutdown
```

Interface Address Verification
------------

Like `ipconfig` / `ip a` commands on a client, you can verify IP addresses on a switch or a router.

```
Switch> en
Switch# show ip interface brief
```


Chapter 3 - Network Protocols and Communications
===========

Rule Establishment for Protocols
--------

* An identified sender and receiver
* Common language and grammar
* Speed and timing of delivery
* Confirmation or acknowledgement requirements

__Network communication protocols share these traits__

* Message encoding
* Message Formatting and Encapsulation
* Message size
* Message Timing
* Mesage Delivery Options

Protocol Interaction
------

* __HTTP:__ Application protocol that governs the way a webserver and a webclient interact. HTTP relies on other protocols to govern how the messages are transported between the client and the server.
* __TCP:__ The transport protocol that manages the individual converstaions. TCP divides the HTTP messages into smaller pieces. (Segments) TCP is also responsible for controlling the size and rate at which mesages are exchanged between the server and clients.
* __IP:__ Takes formatted TCP segments, encapsulates them into packets, assignes the appropiate IP and delivers them to the destination.
* __Ethernet:__ Communication over Data Link and physical transmission on the network media. Network Access protocols take the packets from IP and formats them to be transmitted over the media.


TCP/IP Protocol Suite
------
The TCP/IP Protocol Suite includes many protocols. 
* Applictaion Layer
	- Name System
		* DNS - Domain Name System
	- Host Config
		* BOOTP - Bootstrap Protocol
		* DHCP - Dynamic Host Configuration Protocol
	- Email
		* SMTP - Simple Mail Transfer Protocol
		* POP - Post Office Protocol
		* IMAP - Internet Message Access Protocol
	- File Transfer
		* FTP - File Transfer Protocol
		* TFTP - Trivial File Transfer Protocol
	- Web
		* HTTP - Hypertext Transfer Protocol
* Transport Layer
	- UDP - User Datagram Protocol
	- TCP - Transmission Control Protocol
* Internet Layer
	- IP - Internet Protocol
	- NAT - Network Address Translation
	- IP Support
		* ICMP - Internet Control Message Protocol
	- Routing Protocols
		* OSPF - Open Shortest Path First
		* EIGRP - Enhanced Interior Gateway Routing Protocol
* Network Access Layer
	- ARP - Address Resolution Protocol
	- PPP - Point to Point Protocol
	- Ethernet
	- Interface Drivers

TCP/IP Communication Process
-----
1. Webserver prepares the HTML page to be sent
2. A header is added to the HTML data which contains various information like the HTTP version and the status code
3. The application protocol delivers the data to the transport layer
4. The IP information is added to the front of the TCP information. IP assigns the source and destination address. This is known as an IP packet.
5. The Ethernet protocoll adds information on both ends, creating a data link frame. This frame is delivered to the nearest router along the path towards the web client. This router removes the Ethernet information, analyzes the IP packet, determines the best path for the packet, inserts the packet into a new frame, and sends it to the next neighboring router towards the destination. Each router removes and adds new data link information before forwarding the packet.
6. This data is now transported through the internetwork, which consists of media and intermediary devices.


The OSI Reference Model
----

| Layer | Description|
|:-------:|:------------|
| Application | Contains protocols used for process to process communications |
| Presentation | Provides for common representation of the data transferred between application layer services |
| Session | Provides services to the presentation layer to organize its dialogue and to manage data exchange |
| Transport | Defines services to segment, transfer and reassemble the data for individual communications between end devices |
| Network | Provides services to exchange individual pieces of data over the network between identified devices |
| Data Link | Describes methods for exchanging data frames between devices over a common media |
| Physical | Describes the mechanical, electrical, functional and procedural means to activate, maintain and deactivate physical connections for bit transmission to and from a network device |




The TC/IP Model
----- 

| Name | Description |
|:----:|:----|
| Applictaion | Represents data to the user, plus encoding and dialog control |
| Transport | Supports communications between various devices across diverse networks |
| Internet | Determines the best path through the network |
| Network Access | Controls the hardware devices and media that make up the network |