# Procedures of Reset/Restoration

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

## Password Restore

