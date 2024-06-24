---
title: IT Helper - Bug

---


# Linux
# Setting Linux
- Nord theme terminal: https://github.com/nordtheme/gnome-terminal (vao setting -> profile chuyen sang nord)
- Nordic theme, icon: https://www.gnome-look.org/p/1267246/ và WhiteSur-dark-nord
- tweaks, extension manager
---
- google drive: ~/.themes: folder - Nordic (https://drive.google.com/drive/u/1/folders/1gbXBTUPjf4kY0Kx4e-i_iUMoRAHVHiRz)
                ~/    .icons -Nordzy
# Delete app
- check app thuộc dpkg (apt) hay snap (apt-get): dpkg -l
- dpkg:
    - sudo dpkg --purge name

- snap: 
    - sudo apt-get purge + name
    - sudo apt-get autoremove + name
# Bug Wifi
- Not found symbol wifi in setting: sudo apt-get install --reinstall network-manager network-manager-gnome

# sudo apt update error ip not found
-  sudo rm /etc/apt/sources.list.d/**impish-security**.list
    -  impish-security: tên package lỗi


# Windows
# We couldn't any driver
- mở bios check service tag (cột 2) trong bios setup
- vào dell driver search tên service => install intel rapid storage driver
# How to fix the RAW flash drive\
https://recoverhdd.com/blog/how-to-fix-the-raw-flash-drive.html

What is the RAW file system and what causes this error?
Before fixing or preventing a RAW error, you must first understand what a RAW error is. RAW is a file system error which means that the operating system cannot recognize the file system type or that it cannot access the file system of the flash drive or memory card. Some of the common reasons for your file system to become a RAW are improper flash drive removal, file system corruption, virus attack, lots of bad sectors, etc.

The other possible cause of the RAW error is a file system incompatible with OS Windows. For example, if you connect a flash drive formatted in APFS (Apple File System) to your PC – Windows will show that you are using a RAW flash drive since it does not support this type of file system.

It is worth noting that OS Windows supports the following file systems:

FAT;
NTFS;
exFAT;
Live File System;
ReFS;

Accordingly, if you know that your flash drive uses a different file system – then it is best to use a third-party utility to open it in Windows, otherwise, you will 100% get a RAW drive. For example, to open a memory card formatted in APFS you can use a program from Paragon Software, which adds support for this file system to OS Windows. You can download the program from the official site.

In addition, the following messages also tell you that your flash drive has become RAW:

“You need to format the flash drive before you can use it. Do you want to format it?”

“The flash drive is not formatted. Do you want to format it?”

“There was an error accessing the flash drive X: the USB drive is not formatted”

“The file system type is RAW. CHKDSK is not available for RAW drives”

When your USB flash drive’s file system changes to RAW, it becomes unreadable, which means that you cannot view the contents of the pen drive using standard tools. It means that you will not be able to access the data stored on it.

Since a RAW error is a file system fault, it is possible to repair it without losing the information. The process of repairing a RAW memory card is described in detail in the following paragraphs of this article.
### Converting a RAW flash drive using the command prompt
This method, as well as the previous one, should be used only if your flash drive did not contain any important information, because, as in the previous method, all the data on the flash drive will be destroyed.

The point is that sometimes an error occurs when formatting a USB drive or memory card using Windows Explorer:

Windows was unable to complete the format

In this case, the best way to format the flash drive is to use the diskpart utility, which can be accessed from the command prompt or Windows PowerShell.

To use this method you should:

 Right-click on the “Start” button and select “Run” (you can also use the “Win+R” key combination)
How to fix the RAW flash drive?
In the window that appears, type “cmd” and press “Enter”How to fix the RAW flash drive?
You will see the command prompt window. Type the “diskpart” command and press “Enter” again.How to fix the RAW flash drive?
After successfully running the “**`diskpart`**” utility, enter the following commands one by one (and press “Enter” after each command to execute it):
**`list disk`**
– this command displays all the drives connected to your computer;

**`select disk n`**
– this command selects the flash drive you want to work with. n should be replaced by the number of the flash drive containing the RAW error;

**`clean`**
– this command completely cleans the previously selected flash drive (destroying the file system and all data);

**`create partition primary`**
– this command will create a new partition on your flash drive;

**`active`**
– this will make your partition active;

**`list partition`**
– to display all the partitions on your flash drive;

**`select partition 1`**
– this command will select the partition you just created for future use;

**`FORMAT OVERRIDE QUICK LABEL=name_volume`**
– this command will format your partition to NTFS (you don’t need to specify file system type as diskpart will format your flash drive to NTFS automatically).

FixedDrv is the name of your recovered pen drive. Accordingly, you can change it to any other name you like;

When the formatting process is complete, the result will look something like this:

### How to fix the RAW flash drive?
To quit the diskpart utility, just type “exit” and press “Enter“

All done. A new file system is created on your flash drive and you can use it again.

Read more:  How to recover lost data from RAID 5 array?
The RAW flash drive is write-protected
Sometimes there are situations when users cannot fix a RAW error using the above-described methods. When they trying to format a flash drive — a notification that the flash drive is write-protected appears.

How to fix the RAW flash drive?
In such cases, you should first extract the data from your flash drive and only then proceed to fix the access error, since because the possibility of losing important information is very high. For this purpose use RS Partition Recovery – professional software for data recovery. The program will automatically determine the file system type and other parameters necessary for successful data recovery. This process is described in detail in the next paragraph of this article.

After the data has been successfully recovered you can proceed directly to the flash drive repairing. The first thing to do is to check if your flash drive has mechanical write protection installed on it and if viruses are blocking access. More information can be found in the article “How to remove write protection from the flash drive” If the instructions from that article did not help, you should use low-level formatting. You can use “HDD Low Level Format Tool” software for that.

If low-level formatting did not help either, most likely your flash drive is nearing the end of its lifespan. The thing is that all flash drives have a limited number of write cycles, and when it is exhausted – the controller, which is built into the USB drive and is responsible for the data distribution switches the drive into “read-only” mode. Sometimes it can happen for other reasons, but either way, it is a clear signal that you should save all important data because the flash drive will fail soon. That is why you should immediately use the instructions on how to extract the data from a RAW flash drive in the next paragraph of this article.