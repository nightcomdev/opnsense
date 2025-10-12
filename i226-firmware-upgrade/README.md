# Intel i226-V firmware upgrade v2.32
This section describe steps and help understand firmware upgrade of Intel i226-V from version 2.17 to 2.32
#### You doing this upgrade on your own risk! Please read all informations before you start.

## Intel i226-V Firmware v2.32 upgrade pack

Link to Firmware upgrade pack for i226-V with image v2.32 (1MB version)

https://github.com/nightcomdev/opnsense/raw/refs/heads/main/i226-firmware-upgrade/i226.firmware.v2.32.zip

This pack contains:
1) This README file
2) Billy Curtis i226-V v2.32 1MB bin
3) Example nvm.cfg file
4) nvmupdate64e util from the Intel 34.0 download bundle
5) Intel doc on using nvmupdate64e

#### Steps to upgrade your ethernet card
1. Copy and unzip files to your OPNsense router
2. Check what card do you have `dmesg |grep igc0` change number if you have multiple cards igc1, igc2...
3. Copy MAC address and paste it in next line without `:`
4. `./nvmupdate64e -b -l nvm.log -m 00E0B468DCBC -f -u -c nvm.cfg`
5. Firmware should be upgraded, REBOOT

## More info about i226-V firmware upgrade
#### Topic on OPNsense forum
https://forum.opnsense.org/index.php?topic=48695.0

#### For more informations about upgrading i226-V firmware go to repository of BillyCurtis

https://github.com/BillyCurtis/Intel-I226-V-NVM-Firmware

#### How do use Intel® Ethernet NVM Update Tool

http://tmei-net.com/technology-sharing/how-do-use-intel-ethernet-nvm-update-tool.html
