# Procedures of Reset/Recovery

## Factory Configuration Reset
Approach: Delete configuration files and VLAN infomation.
1. Under Global Configuration mode, issue `write erase`.  
2. `reload` switches/routers **without save** when prompt for configuration modified.
3. Reset vlan information by issuing `delete flash:vlan.dat`.
4. `reload` devices.
```
switch#write erase                  //reset configurations (not clear the boot variables, such as config-register and boot system settings)
switch#dir flash:                   //check vlan.dat file before deletation
switch#delete falsh:vlan.dat        //reset vlan configuration
switch#reload                       //reboot devices
```

## Password Recovery
Approach: Using physical button or break-key signal to break normal boot. Then disable the existing configuration file be loaded by boot loader and instead loading factory-default configuration to enter the device's OS. Finally, under global config mode, reload the exsiting configuration file and change those forgotten passwords.
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
4. Issue the `flash_init` command.
5. Issue the `load_helper` command.
6. Issue the `dir flash:` command to check if the configuration file _config.text_ is there.
7. Rename the confiuration file by command `rename flash:config.text flash:config.old`.
8. Issue `boot` command to boot the system.
9. After the device boot, enter "n" at the prompt to abort the initial configuration dialog.
10. Enter the global configuration mode.
11. Issue `rename flash:config.old flash:config.text` to rename the configuration file wiht its original name.
12. Copy the configuration file into memory by `copy flash:config.text system:running-config`.
13. Overwrite the current passwords that you don't know. For example, command `enable password <password-strings>`.
14. Save the running configuration by `write memory`.
