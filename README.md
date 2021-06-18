# MakAir Files

## Abstract

This repository contains all large files, that do not fit in other repositories. Those large files are stored in [releases](https://github.com/makers-for-life/makair-files/releases), as there is no file size limit there.

These files can be useful when building a MakAir ventilator, eg. to quick flash a fully-working Linux system image to the Raspberry Pi for the Control UI.

## Control UI

### Image flash instructions

To flash the provided Control UI system image on a target SD card, please first make sure that:

1. You run a Linux-based or macOS system;
2. Your SD card is at least 8GB (the image will fill 8GB of blocks, so it is optimized for 8GB cards but also works with eg. 16GB, 32GB, etc. cards);

Please follow the following steps to flash the provided images (on macOS):

1. Note the mounted disk number with: `diskutil list` (for instance: `/dev/rdisk2`);
2. Unmount all disk partitions, but do not eject them (can be done with Disk Utility in macOS);
3. Flash the image to the target disk with: `bunzip2 -dc ./makair-control-ui-rpi-production-v2_3_0.img.bz2 | sudo dd bs=1m of=/dev/rdisk2`;
4. Eject the disk once flashed, and insert it in a MakAir ventilator;

Note that if connecting to the system over SSH, the user would be `alarm` and password: `alarmpi`. Once logged-in, the root password will be: `makair`.

_⚠️ Make sure to prepare your own image for production machines, and change this root password!_

### Image update instructions

To update the Control UI contained in an existing image, please use a Linux-based system and follow those instructions:

1. Download the compressed image from [the releases](https://github.com/makers-for-life/makair-files/releases) on your Linux computer (for instance: `makair-control-ui-rpi-production-v2_3_0.img.bz2`);
2. Open a terminal, and upgrade to root: `sudo su` or `su`;
3. Download your target Control UI version: `wget https://github.com/makers-for-life/makair-control-ui/releases/download/v2.3.0/v2.3.0-linux-armv7l.tar.gz`;
4. Extract the Control UI: `tar -xf v2.3.0-linux-armv7l.tar.gz && rm v2.3.0-linux-armv7l.tar.gz`;
5. Extract the compressed image (will take some time): `bunzip2 makair-control-ui-rpi-production-v2_3_0.img.bz2`;
6. Create a mount point for the image: `mkdir image`;
7. Mount the image ext4 system partition: `sudo mount -o loop,rw,sync,offset=105906176 makair-control-ui-rpi-production-v2_3_0.img ./image` (offset can be acquired with `fdisk -l [image].img`);
8. Go to the directory containing the Control UI binary: `cd ./image/home/alarm/`;
9. Remove the previous Control UI binary: `rm ./makair-control-ui`;
10. Copy the new Control UI there: `cp ../../../makair-control-ui/makair-control-ui ./`;
11. Unmount the updated image partition: `cd ../../../; umount image/`;
12. Now, you can flash the updated image to a micro SD card and archive it to [the releases](https://github.com/makers-for-life/makair-files/releases) (bzip2-compressed);
