# ThinkPad T440P Hackintosh OpenCore
![Screen Shot 2021-02-11 at 10 59 53 PM](https://user-images.githubusercontent.com/72950020/107709771-abb02a80-6cbd-11eb-87f6-ad79257461e1.png)

## Intro and Links

Hello, in October 2020 I aquired a ThinkPad T440P to use as my daily driver.

I got the idea to get T440P from Sebi's Random Tech channel on Youtube (https://www.youtube.com/user/SebisGameReviews).

I also learned alot about T440P and got the idea to hackintosh from Wolfgang's Channel (https://www.youtube.com/channel/UCsnGwSIHyoYN0kiINAGUKxg).

(i recomend checking both of these out if you are a T440P owner)

On the first day I got the laptop I installed Catalina 10.15.7 with Clover using jloisel's guide (https://github.com/jloisel/t440p).

After doing some research I wanted to try OpenCore so that i could use Big Sur, however I also wanted to see if i could improve performance and or functionality on Catalina.

I also wanted to try all the other T440P builds around so i have tried alot

Swanux - https://github.com/swanux/t440p

LighterEB - https://github.com/lighterEB/ThinkPad-T440p

2000c43 - https://2000c43.com/blog/the-perfect-laptop

Sniki - (https://github.com/Sniki/Lenovo-Thinkpad-T440P-Clover)   
(https://www.tonymacx86.com/threads/guide-lenovo-thinkpad-t440p-opencore-0-5-9.299277/)

I noticed that all Clover builds used FakeSMC and all OpenCore builds used VirtualSMC, personally i had some issues with VirtualSMC so i tried using FakeSMC with OpenCore and found it to work much better

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

If you wish to dual boot please use another drive for windows and i suggest disabling the drive with MacOS on it in BIOS while using windows to avoid windows update fucking with the EFI or other parts of MacOS

Follow this guide to learn how create a USB installer - https://www.macworld.co.uk/how-to/bootable-mac-installer-3575875/

### Copy EFI folder to USB

Copy the content of the EFI folder provided here with your own Serial ETC on your USB flash drive EFI partition. The EFI partition is usually hidden. You can use OpenCore Configurator, Clover Configurator, and a few other tools to mount it)

### Install macOS

Install macOS by booting on the USB key. It takes about 30min. The computer will restart multiple times. Make sure to select Install macOS ... each time. Once installed, choose to boot from local drive in Clover boot menu.
What's next?

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
this is the Apple card which comes from Macbook Airs, its 2x2 same as thinkpad, and fits alot easier than one would think, in order to fit the card you must remove the bottom cover, which requires removing the keyboard and palmrest, however once done wifi does not require any extra kexts and works flawlessly with NSS:2 link rate 867 on Catalina and Big Sur

Status | Finished
-- | --
Test time | 10.17 seconds
Transferred bytes | 344.93 MB uploaded
Speed | 284.61 Mbps

## Other

regarding dock, sorry i dont have one i cant test

SD port is not working for me

wake from sleep using bluetooth devices with BCM94360CS2 is not working.


