## Proc
Proc - is a directory with virtual files. They don't exist and created every time you boot your system.

1. In order to get memory do `cat /proc/meminfo`
2. In order to get kernel version do `cat /proc/version`
3. In order to get cpu info type `car /proc/cpuinfo`


Proc directory contains a lot of numeric subfolder. Every subfolder
is a separate process. Inside of this process subfolder you can find
1. `cmdline` - Command that started the process
2. `status` - Status of process
