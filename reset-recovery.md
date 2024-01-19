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
switch#reload                       //reboot devices
```

## Password Recovery - Bypass loading *start-config*
### Approach #1 - Rename *start-config* file
Using physical button or break-key signal to break normal boot. Then disable the existing configuration file be loaded by boot loader and instead loading factory-default configuration to enter the device's OS. Finally, under global config mode, reload the exsiting configuration file and change those forgotten passwords.
1. Connect a PC/terminal to console port of a switch/router.
2. Unplug the power cable to the switch/route.
3. Power on the switch/router and bring it to the `switch:` prompt by breaking normal boot-up. There are two ways:   
    - Physical buttons: Hold down the _mode_ button located on the front panel, while reconnect the power cable. 
        - Catalyst 3560, 3750: Release the _mode_ button after approximately 15 seconds when the _SYST_ LED turns solid green. When release the _mode_ button, the _SYST_ LED blinks green.
        - Catalyst 2900XL, 2500XL: Release the _mode_ button when the LED above _port1x_ goes out. 
        > Notes: The break methods may differ among different cisco models. Always referring to manuals.
    - Software break-key: The devices boot loader detects a _break-key_ input to stop the automatic boot sequence for the password recovery purposes.
        - Hyperterminal: Press _Ctrl_ + _Break_ for the break-key.
        - Unix terminal: Press _Ctrl_ + _C_ for the break-key.
4. Issue `flash_init` command.
5. Issue `load_helper` command.
6. Issue `dir flash:` command to check if the configuration file _config.text_ is there.
7. Rename the confiuration file by command `rename flash:config.text flash:config.old`.
8. Issue `boot` command to boot the system.
9. After the device boots up, enter "n" at the prompt to abort the initial configuration dialog.
10. Enter global configuration mode.
11. Issue `rename flash:config.old flash:config.text` to rename the configuration file back to its previous name.
12. Copy the configuration file into memory by `copy flash:config.text system:running-config`.
13. Overwrite the current passwords that you don't know. For example, command `enable password <password-strings>`.
14. Save the running configuration by `write memory`.
### Approach #2 - Reset Register code
```
rommon>confreg 0x2142
rommon>reset
```

## `Break` key not work
A solution for fixing `Break` key of keyboard not work.
1. Set terminal session's spped rate to **1200**; (keep other settings as default: *no parity, 8 data-bits, 1 stop bit, no flow control*)
2. Power cycle the devices and press/hold `Space` key for 10-15 seconds in order to generate a signal similiar to *break* sequence;
3. Reconnect terminal session and set speed rate back to **9600**.
Now, you are in ROM Monitor mode.
