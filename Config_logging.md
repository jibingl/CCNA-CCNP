# Configure Cisco Devices Logging

### Enable or disable logging
Notes: Enable and disable logging on Cisco devices. By defualt, logging is enabled. Disabling message logging is actually stop a logging process to which log messages are sent. So, all log messages need to be written to the console instead, which will slow dwon devices.
Disable logging:
```
configure terminal
no logging on                           #Disable logging
logging on                              #Re-enable logging
```
---
### Display log messages onto different destination devices/screens
```
configure terminal
logging buffered [size][level]          #Logs messages to an internal buffer 
logging [host]                          #Logs messages to a external syslog server
logging console [level]                 #Display messages to the console (enabled by defualt)
terminal monitor                        #Display messages to a non-console terminal (eg,SSH) during the current session
```
---
### Configure logging messages to an external Syslog server
##### Configue a Unix Syslog daemon:
1) Log in as root.
2) Confirm the Unix Syslog daemon accepts syslog packets from network.
3) Add a line such as the following to the file etc/syslog.conf: `local7.debug /usr/adm/logs/cisco.log`
4) Creat the log file:
  ```
  $touch /usr/adm/log/cisco.log
  $chmod 666 /usr/adm/log/cisco.log
  ```
5) Ensure the syslog daemon reads the new changes:
  ```
	$kill -HUP `cat /etc/syslog.pid`
  ```
##### Configure Cisco devices:
  ```
  configure terminal
  logging [host]
  logging trap [level0-7]
  logging facility [faxility-type]          #The default-type is local7
  end
  no logging                                #Remove a syslog server
  no logging trap                           #Disable logging to syslog servers
  ```
