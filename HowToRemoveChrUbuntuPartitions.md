# How to remove ChrUbuntu partitions (restore chromebook disk)

Everywhere it says that to go back to original disk partition you can "Recover your Chromebook" (https://support.google.com/chromebook/answer/1080595?hl=en). That didn't work for me because I didn't had enough space in my chromebook to create the recovery usb disk. 

What did worked for me? I run some commands to invert the changes that "ChrUbuntu's installation script" made.

The [ChrUbuntu's script](http://goo.gl/9sgchs) added new partitions by running the following commands:
```
# the original partition is changed to be smaller
cgpt add -i 1 -b $stateful_start -s $stateful_size -l STATE ${target_disk}
 
# this creates a new partition called "kernc"
cgpt add -i 6 -b $kernc_start -s $kernc_size -l KERN-C ${target_disk}
 
# this creates a new partition called "rootc"
cgpt add -i 7 -b $rootc_start -s $rootc_size -l ROOT-C ${target_disk}
```

So, I deleted *kernc* and *rootc* partition. Then I restored the #1 partition to its original state. This is what I did: 

```
#delete partition #6 "kernc" 
cgpt add -i 6 -t unused /dev/sda 
#delete partition #7 "kernc" 
cgpt add -i 7 -t unused /dev/sda 
#restore partition #1 size 
cgpt add -i 1 -s 282624 -b 22605823 /dev/sda 
```

WARNING! You are changing your disk partitions!! You better know what you are doing and don't blame me :D

Chromebook Model: HP Chromebook 14
