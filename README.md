Creates a **Rocky Linux** usb drive that will boot on Apple M-series systems

<img src="https://github.com/user-attachments/assets/bfc7285c-de6b-4860-9f44-81bb0ccd4657" width=65%> 


**This image was built on a Fedora aarch64 system   

**Fedora Package Install**  
```dnf install arch-install-scripts bubblewrap gdisk mkosi pandoc rsync systemd-container```

#### Notes

- The root password is **rocky**  
- The ```qemu-user-static``` package is needed if building the image on a ```non-aarch64``` system  
-This project will work with mkosi versions less then or equal to mkosi v23 If needed, you can always install a specific version via pip
python3 -m pip install --user git+https://github.com/systemd/mkosi.git@v23

To build a minimal Rocky Linux image and install it to a usb drive, simply run:
```
./build.sh -d /dev/sda
```

**note:** substitute ```/dev/sda``` with the device id of your usb drive

If you've previously installed this Rocky Linux image to the usb drive, you can wipe the drive and install a new image without having to repartition/reformat the drive by providing the `-w` argument
```
./build.sh -wd /dev/sda
```

Once the drive is created, you can locally mount, unmount, or chroot into the usb drive (which contains 3 partitions) to/from ```mnt_usb/``` with
```
./build.sh mount
./build.sh umount
./build.sh chroot
```
**note:** mounting the usb drive is useful for inspecting the contents of the drive or making changes to it

To boot the usb drive on an apple silicon system:

Boot to Asahi Linux on the internal drive and add the usb drive to the grub menu  
```
[root@m1 ~]# grub2-mkconfig -o /boot/grub2/grub.cfg 
Generating grub configuration file ...
Found Fedora Linux Asahi Remix 42 (Forty Two [Adams]) on /dev/nvme0n1p7
Found Rocky Linux 9.6 (Blue Onyx) on /dev/sda3
```
You should now see the `/dev/sda3` entry in the main grub menu  
If you don't see the grub menu at all or if the text is garbled, then ensure these options are set in `/etc/default/grub`
and then run `grub2-mkconfig -o /boot/grub2/grub.cfg` on the internal drive
```
GRUB_TIMEOUT=5
GRUB_TIMEOUT_STYLE="menu"
GRUB_TERMINAL_OUTPUT="gfxterm"
GRUB_FONT=/boot/grub2/fonts/unicode.pf2
```

**Setting up WiFi**

To connect to a wireless network, use the following sytanx:
```nmcli dev wifi connect network-ssid```

An actual example:
```nmcli dev wifi connect blacknet-ac password supersecretpassword```
