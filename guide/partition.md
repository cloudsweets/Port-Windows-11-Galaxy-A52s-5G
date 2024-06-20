# Installation

## Partitioning your device

## Notes:

> [!WARNING]  
> Do not run the same command twice unless specified.
> 
> DO NOT REBOOT YOUR PHONE! If you think you made a mistake, ask for help in the [Telegram chat]().
> 
> Do not run all commands at once, execute them in order! 
>
> YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!
> 

## Prerequisites

- Have the bootloader unlocked

- modified [orangefox]() recovery

- Have downloaded the [Platform Tools](https://developer.android.com/tools/releases/platform-tools)

- Please download the [uefi]() file for A52s

- Please download the [driver]() for A52s

- You need the [vbmeta.tar]() file to install orangefox recovery

- [Odin3](https://gitlab.com/Ryzen5950XT/odin_dl/-/raw/main/Odin3_v3.14.4.zip?inline=false) is required

#### You need to boot into orangefox recovery

>1. Your device needs to boot into download mode
>
>2. With the device powered off, hold **Volume Up** and **Volume Down** and connect the USB cable to your PC.
>
>3. Click **AP**, select the **orangefox** **recovery** file, and click the **Start** button to install.
>
>4.  Then, it will automatically reboot into download mode again. There will be an error message, but ignore it. Click **AP** in Odin3, select **vbmeta.tar**, and click the **Start** button.
>
>5. As soon as the device turns off, press the **volume up** button and **power** button simultaneously to boot into **orangefox recovery**.

#### Unmount all partitions
- Click the menu button in orangefox recovery. Click the **Mount button**. Please **uncheck** the **data button**

## Start the ADB shell
```sh
adb shell
```
## Create Partitions

### Start parted
```sh
parted /dev/block/sda
```

### Delete the `userdata` partition
> You can make sure that 34 is the userdata partition number by running
>  `p`
```sh
rm 34
```

### Create partitions

#### For 128Gb models:

- Create the ESP partition (571MB)
```sh
mkpart esp fat32 13.2GB 13.8GB
```

- Create the main partition where Windows will be installed to (60GB)
```sh
mkpart win ntfs 13.8GB 73.8GB
```

- Create Android's data partition (53.6GB)
```sh
mkpart userdata ext4 73.8GB 127GB
```

### Make ESP partiton bootable so the EFI image can detect it
```sh
set 34 esp on
```

### Quit parted
```sh
quit
```

### Reboot to orangefox recovery

### Start the shell again on your PC
```cmd
adb shell
```

### Format partitions
-  Format the ESP partiton as FAT32
```sh
mkfs.fat -F32 -s1 /dev/block/bootdevice/by-name/esp -n ESPA52SXQ
```

-  Format the Windows partition as NTFS
```sh
mkfs.ntfs -f /dev/block/bootdevice/by-name/win -L WINA52SXQ
```

- Format data
Go to Wipe menu and press Format Data, 
then type `yes`.

### Check if Android still starts
just restart the phone, and see if Android still works

## [Next step: Install Windows]()