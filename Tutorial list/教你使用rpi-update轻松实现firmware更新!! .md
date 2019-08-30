# 教你使用rpi-update轻松实现firmware更新!! 

rpi-update是老外开发的一个更新树莓派firmware的工具，很方便也很好用，推荐给大家! 

$部分是需要输入的： 

```
$ sudo apt-get update  
$ sudo apt-get install ca-certificates 
$ sudo apt-get install git-core 
$ sudo wget  -O /usr/bin/rpi-update 
$ sudo chmod +x /usr/bin/rpi-update 
$ sudo rpi-update 
Raspberry Pi firmware updater by Hexxeh, enhanced by AndrewS 
Performing self-update 
Autodetecting memory split 
Using ARM/GPU memory split of 192MB/64MB 
We're running for the first time 
Setting up firmware (this will take a few minutes) 
Using SoftFP libraries 
/opt/vc/sbin/vcfiled: error while loading shared 
libraries:libvchiq_arm.so: cannot open shared object file: No such file 
or directory 
If no errors appeared, your firmware was successfully setup 
A reboot is needed to activate the new firmware 
$ sudo ldconfig 
$ sudo rpi-update 
Raspberry Pi firmware updater by Hexxeh, enhanced by AndrewS 
Performing self-update 
Autodetecting memory split 
Using ARM/GPU memory split of 192MB/64MB  
Updating firmware (this will take a few minutes) 
Your firmware is already up to date 
$ sudo reboot
```

