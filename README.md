# ThinkPad T440P Hackintosh OpenCore
![IMG_2865](https://user-images.githubusercontent.com/72950020/115569903-8f300000-a2b5-11eb-9c69-2be2cab4b6a5.JPG)
![Screen Shot 2021-06-07 at 11 30 01 PM](https://user-images.githubusercontent.com/72950020/121499806-9fbb4780-c9d5-11eb-8974-f283c4d70334.png)


## Intro and Links

CURRENT RELEASE - v0.3-beta2, see releases page for older releases here - https://github.com/VinylNerd/ThinkPad-T440P-OpenCore/releases

Hello, in October 2020 I aquired a ThinkPad T440P to use as my daily driver.

I got the idea to get T440P from Sebi's Random Tech channel on Youtube (https://www.youtube.com/user/SebisGameReviews).

I also learned alot about T440P and got the idea to hackintosh from Wolfgang's Channel (https://www.youtube.com/channel/UCsnGwSIHyoYN0kiINAGUKxg).

(i recomend checking both of these out if you are a T440P owner)

On the first day I got the laptop I installed Catalina 10.15.7 with Clover using jloisel's guide (https://github.com/jloisel/t440p).

After doing some research I wanted to try OpenCore so that i could use Big Sur, however I also wanted to see if i could improve performance and or functionality on Catalina.

I also wanted to try all the other T440P builds around so i have tried alot.

Swanux - https://github.com/swanux/t440p

LighterEB - https://github.com/lighterEB/ThinkPad-T440p

2000c43 - https://2000c43.com/blog/the-perfect-laptop

Sniki - (https://github.com/Sniki/Lenovo-Thinkpad-T440P-Clover)   
(https://www.tonymacx86.com/threads/guide-lenovo-thinkpad-t440p-opencore-0-5-9.299277/)

I noticed that all Clover builds used FakeSMC and all OpenCore builds used VirtualSMC, personally i had some issues with VirtualSMC so i tried using FakeSMC with OpenCore and found it to work much better

EDIT/NOTE : as of v0.3-beta1 these builds will be switching to VirtualSMC untill further notice, however the build remains unique in that it uses the ACPIbatterrymanager kext rather than SMCbattery manager, this allow the laptop to be used in clamshell mode without power.

## BIOS SETTINGS 

The bios must be properly configured prior to installing MacOS.

In Security menu, set the following settings:

    Security > Security Chip: must be Disabled,
    Memory Protection > Execution Prevention: must be Enabled,
    Internal Device Access > Bottom Cover Tamper Detection: must be Disabled,
    Anti-Theft > Current Setting: must be Disabled,
    Anti-Theft > Computrace > Current Setting: must be Disabled,
    Secure Boot > Secure Boot: must be Disabled.

In Startup menu, set the following options:

    UEFI/Legacy Boot: Both,
    UEFI/Legacy Priority: UEFI First,
    CSM Support: Yes.

Now you can go through the install.

## Generating your own serial and Editing ROM

using GenSMBIOS (https://github.com/corpnewt/GenSMBIOS)
generate a serial for MacBookPro11,1

 I advise against 11,2 as the usb mapping is slightly different.

use Plist edit pro, or something else to manually enter the details in the config where it sais YOUR STUFF HERE (as shown in photo) SystemSerialNumber, MLB, and UUID

![Screen Shot 2021-02-02 at 5 41 49 PM](https://user-images.githubusercontent.com/72950020/106641108-bf051c80-657e-11eb-931e-d454f765175a.png)

You should also edit your ROM to match the MAC address of your ethernet adapter, as per the opencore guide, however i personally am not sure if it matters.

## CREATING YOUR INSTALLER USB, OR USING THIS EFI


If you already have MacOS installed, i recomend trying this EFI on a USB first, in the boot menu you will see a clearnvram option, choose this, and then the shutdown option. if it works for you then you can proceed to move it to your main drive

If you don't have MacOS installed, we will cover how to install MacOS on your system.

If you wish to dual boot please use another drive for windows and i suggest disabling the drive with MacOS on it in BIOS while using windows to avoid windows update messing with the EFI or other parts of MacOS

Follow this guide to learn how create a USB installer - https://www.macworld.co.uk/how-to/bootable-mac-installer-3575875/

### Copy EFI folder to USB

Copy the content of the EFI folder provided here with your own Serial ETC on your USB flash drive EFI partition. The EFI partition is usually hidden. You can use OpenCore Configurator, Clover Configurator, and a few other tools to mount it)

### Install macOS

Install macOS by booting on the USB key. It takes about 30min. The computer will restart multiple times. Make sure to select Install macOS ... each time. Once installed, choose to boot from local drive in Clover boot menu.


To finish the setup, you need to:

Copy EFI folder from USB flash drive to local drive EFI partition (like you did for the USB drive). It will make the local drive bootable (so you can get ride of the USB drive now),

You're done! Reboot and enjoy macOS on your Thinpad T440p.

## WIFI with T440P

As someone who has worked doing Wi-Fi for hotels i'm fairly picky about the quality of my wireless. this current build does not include and kexts for other wirless cards as im using apple wireless card, you will need to add your own kexts for these cards, i recomend learning more here - https://dortania.github.io/Wireless-Buyers-Guide/Kext.html#broadcom

#### 7260NGW
When i first got the T440P it had Intel 7260NGW 2x2 AC Card which i was able to make work with Wifi and Bluetooth. however the Wifi was not good, 2.4GHz was working but not properly, 5GHz was barely working, bluetooth worked but airdrop did not.

#### DW1820A
This was the first card i tried to use after the intel, i believe 2.4GHz and 5GHz worked, bluetooth worked, but airdrop did not.

#### BCM94360NGW
this was the second card i tried to use, 2.4GHz and 5GHz gave packet loss and maxed out 434mbps NSS:1, bluetooth worked, airdrop worked, wifi was not acceptable.

#### DW1560
this was the third card i tried to use, 2.4GHz and 5GHz work great but max out 434mbps, bluetooth works, airdrop works but is to slow for me.

#### DW1830
this was the fourth card i tried to use, the card is to big so requires being physically cut to fit into the laptop, i made a cut to the card and was able to get it in but it didnt perform as i was likign and didnt show AC link rates so rather than doing more work to fit it properly i stopped, its possible if cut down properly it could work with a 3x3 antenna solution, but because of the price of the card i wouldnt suggest this

#### BCM94360CS2
this is the Apple card which comes from the Macbook Air, its 2x2 same as thinkpad, and fits alot easier than one would think, in order to fit the card you must remove the bottom cover, which requires removing the keyboard and palmrest, however once done wifi does not require any extra kexts and works flawlessly with NSS:2 link rate 867 on Catalina and Big Sur

Status | Finished
-- | --
Test time | 10.17 seconds
Transferred bytes | 344.93 MB uploaded
Speed | 284.61 Mbps


#### BCM94360CS
this is the Apple card which comes from the Macbook Pro, its 3x3 same , and is bigger than the 2x2 card, in order to fit the card you must make a small cut in the case after removing the bottom cover, which requires removing the keyboard and palmrest, however once done wifi does not require any extra kexts and works flawlessly with NSS:3 link rate 1300 on Catalina and Big Sur

Status | Finished
-- | --
Test time | 10.04 seconds
Transferred bytes | 377.21 MB uploaded
Speed | 315.24 Mbps


![IMG_3582](https://user-images.githubusercontent.com/72950020/108794430-a389a200-757d-11eb-88a9-052b9151f2ca.jpg)



## Other

### Voltage Shift

to use voltage shift you must first download it here https://github.com/sicreative/VoltageShift 
once downloaded unpack the 1.25 version.

inside this there will be a voltageshift.exec file, this is all you need, place this file wherever you want and execute the command from there, for example i have a folder named "voltageshift" in my documents folder with the exec file inside of it, so i open terminal and type "cd Documents/voltageshift" and then you can play around with entering commands, but be careful

if you wish to skip toying and unlock your CPU for its max potential i recomend running this command

"sudo ./voltageshift buildlaunchd -70 -70 -70 -70 -70 -70 1 55 80 1 160"

it will yeild this results automatically everytime upon boot

CPU voltage offset: -70mv
GPU voltage offset: -70mv
CPU Cache voltage offset: -70mv
System Agency offset: -70mv
Analogy I/O: -70mv
Digital I/O: -70mv
CPU BaseFreq: 2500, CPU MaxFreq(1/2/4): 3500/3400/3300 (mhz)  PL1: 55W PL2: 80W 
CPU Freq: 2.5ghz, Voltage: 0.7955v, Power:pkg 18.74w /core 8.73w,Temp: 54 c
Connor@Connors-MBP voltageshift % 

### YogaSMC 

Yoga SMC is now partially working as of v0.3-beta1, fan control is working but i can't get DYTC to work, if anyone can help feel free to open an issue

to download go here - https://github.com/zhen-zen/YogaSMC/releases

all you need to download is YogaSMC-App-Release.dmg

once downloaded place in applications folder, to enable control of fan down to stopped go into preferences>think>allowfanstop

any questions or issues please create a ticket

### Dock

Dock is working for usb but using HDMI/DP/DVI/VGA causes kerel panic on sleep/shutdown/reboot. however with the current build uploaded it will cause sleep issues ontop of this, i will upload a new build with the fix once OpenCore 0.6.7 is released in march.

### SD

SD port is not working for me, working fine in windows tho.

### Sleep

sleep wake works as normal with no dock, with dock wake from USB devices will not work or due to a ACPI patch that needs to be applied to stop it from constantly waking with the dock, currently an explanation for this is on the v0.2beta1 release page but i will move it here shortly. 

wake from sleep using bluetooth devices with genuine apple wifi cards like BCM943602CS, BCM94360CS, BCM94360CS2, and BCM94360CD DOES NOT, and will nto work AFAIK, please report if you have a different result, cards like the DW1560 can wake from sleep with bluetooth however it can be a pain to get working.


