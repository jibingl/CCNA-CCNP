# Debug Commands
📢 To observing debug messages, logging must be enabled at debuging level (level 7). Issueing `show logging` to check or confirm it.  

- Too much debug output to console can cause the device to hang. Because console output is prioritized ahead of other functions;
- Logging messages (including debug) are sent to console by default;
- Under conditional debug with multiple condistions set, when any one of the conditions (not all conditions) is met, the corressponding debugging messages are displayed;
- `undebug all` DO NOT remove debugging conditions but only disable debugs. The conditions will be applied to future debugs;
- It is different between `show debug` and `show debugging`;

```console
R1# logging console debugging             //Set the console logging of messages at the debugging level (level 7)

R1# debug [options]                       //Syntax
R1# show debug                            //Display what debugs are enabled
R1# show debugging                        //
R1# undebug all                           //"no debug all" is the same

R1# debug ip packet
R1# debug condition interface g0/0        //Specify conditions for debug
R1# debug condition interface g0/1        //Multipule conditions can be defined 
R1# undebug condition all                 //"no debug condition all" works the same

```
