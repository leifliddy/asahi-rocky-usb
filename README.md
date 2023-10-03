Creates a Rocky Linux usb drive that will boot on Apple M1/M2 systems.

**Fedora package install:**  
  
This image was built on a Fedora system


```
dnf install arch-install-scripts bubblewrap gdisk qemu-user-static pandoc rsync systemd-container
```
**note:** ```qemu-user-static``` is only needed if building on a non-```aarch64``` system.  
- Until version 15.x is released for Fedora, install mksoi from git:  
`python3 -m pip install --user git+https://github.com/systemd/mkosi.git@v15.1`

To build a minimal Fedora image and install it to a usb drive, simply run:
```
./build.sh -d /dev/sda
```

**note:** substitute ```/dev/sda``` with the device id of your usb drive

If you've previously installed this Fedora image to the usb drive, you can wipe the drive and install a new image without having to repartition/reformat the drive by providing the `-w` argument
```
./build.sh -wd /dev/sda
```

Once the drive is created, you can locally mount and unmount the usb drive (which contains 3 partitions) to/from ```mnt_usb/``` with
```
./build.sh mount
./build.sh umount
```
**note:** mounting the usb drive is useful for inspecting the contents of the drive or making changes to it

To boot the usb drive on an M1 system, enter the following ```u-boot``` commands at boot time:
```
env set boot_efi_bootmgr
run usb_boot
```
If anyone knows of an easier method -- please let me know

**Setting up WiFi**

To connect to a wireless network, use the following sytanx:
```nmcli dev wifi connect network-ssid```

An actual example:
```nmcli dev wifi connect blacknet-ac password supersecretpassword```
