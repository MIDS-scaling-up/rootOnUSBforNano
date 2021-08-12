# rootOnUSBforNano
Switch the rootfs to an USB SSD on the Nano.  Ideally, we'd boot direclty from the USB drive, but with JetPack 4.6, this isn't working at the moment

This is based on the approach used with the NX.

This script is updated to use sda1.  If your USB drive is on a different device, you'll need to review and update the scripts in this repo, including the ones in the data directory.


This is taken from the NVIDIA Jetson AGX Xavier forum https://forums.developer.nvidia.com/t/how-to-boot-from-nvme-ssd/65147/22, written by user crazy_yorik (https://forums.developer.nvidia.com/u/crazy_yorick). Thank you crazy_yorik!

This procedure should be done on a fresh install of the SD card using JetPack 4.6. Install the USB 3.0 drive Jetson, and format it gpt, ext4, and setup a partition (p1)

Next, copy the rootfs of the eMMC/SD card to the SSD
```
$ ./copy-rootfs-ssd.sh
```

Then, setup the service. This will copy the .service file to the correct location, and install a startup script to set the rootfs to the SSD.
```
$ ./setup-service.sh
```

After setting up the service, reboot for the changes to take effect.

### Boot Notes
These script changes the rootfs to the SSD after the kernel image is loaded from the eMMC/SD card. For the Xavier NX, you will still need to have the SD card installed for booting. As of this writing, the default configuration of the Jetson NX does not allow direct booting from the NVMe.

### Upgrading
Once this service is installed, the rootfs will be on the SSD. If you upgrade to a newer version of L4T using OTA updates (using the NVIDIA .deb repository), you will need to also apply those changes to the SD card that you are booting from.

Typically this involves copying the /boot* directory and /lib/modules/\<kernel name\>/ from the SSD to the SD card. If they are different, then modules load will be 'tainted', that is, the modules version will not match the kernel version.


## Notes
* Tested on Jetson Nano
