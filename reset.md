# Procedures of Reset/Recovery

## Factory Configuration Reset
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
1. Connect a PC/terminal to console port of a switch/router.
2. Unplug the power cable to the switch/route.
3. Power on the switch/router and bring it to the `switch:` prompt,
    - Catalyst 3560, 3750: Hold down the _mode_ button located on the front panel, while reconnect the power cable. Release the _mode_ button after approximately 15 seconds when the _SYST_ LED turns solid green. When release the _mode_ button, the _SYST_ LED blinks green.
    - 
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
