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
Example: `Switch(config)#`. To return to Privileged Exec Mode enter `end` or use `CTRL + Z` command.

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


Check Banner / Message of the Day / MotD
----------------------------------------

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

__Save__
```
Router> enable
Router#
Router# copy running-config startup-config
```






