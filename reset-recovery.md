# Procedures of Reset or Recovery

## Factory Configuration Reset
Approach: Delete configuration files and VLAN infomation.
1. Under Global Execution mode, issue `write erase` to delete both start-configuration and running-configuration.
2. Issuing `delete flash:vlan.dat` to delete/reset vlan configuration. 
3. `reload` switches/routers **without save** when prompt for configuration modified.
```
switch#write erase                  //reset configurations (not clear the boot variables, such as config-register and boot system settings)
switch#dir flash:                   //check vlan.dat file before deletation
switch#delete falsh:vlan.dat        //reset vlan configuration
switch#reload                       //reboot devices without save
```

## Password Recovery - Bypass loading *start-config*
### Approach #1 - Rename *start-config* file
By accessing ROM mode, you may bypass existing start-config file being loaded at the next boot. Then you can get into switch/router without custom configurations and do whatever you need.
1. Connect to console port of the switch/router.
2. Power off the device, then power on and bring it into `switch:` or `rommon>` prompt by breaking normal boot-up process. There are two ways: 
    - Physical buttons: Hold down the _mode_ button located on the front panel, while reconnect the power cable. 
        - Catalyst 3560, 3750: Release the _mode_ button after approximately 15 seconds when the _SYST_ LED turns solid green or off. When release the _mode_ button, the _SYST_ LED blinks green.
        - Catalyst 2900XL, 2500XL: Release the _mode_ button when the LED above _port1x_ goes out. 
        > Notes: The break methods may differ among different cisco models. Always referring to manuals.
    - Software break-key: The devices boot loader detects a _break-key_ input to stop the automatic boot sequence for the password recovery purposes.
        - Hyperterminal: Press _Ctrl_ + _Break_ or send a break signal via menu.
        - Unix terminal: Press _Ctrl_ + _C_ for the break-key.
3. Under recovery mode, find and rename current existing start-config file. Then reboot.
```
switch: flash_init
switch: load_helper
switch: dir flash:
 Directory of flash:/
    5    -rwx    402    <date>    config.text
switch: rename flash:config.text flash:config.old
switch: boot
```
4. After the device boots up, enter galobal configuration mode and name the config file back and load it as running-config.
```
S1(config)# rename flash:config.old flash:config.text
S1(config)# copy flash:config.text system:running-config
```
5. Overwrite the any existing password as you need.

### Approach #2 - Set Register code 0x2142
```
rommon>confreg 0x2142
rommon>reset
```
> Don't forget to set config-register back `0x2102`, the default value, after erasing configuration.

