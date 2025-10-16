# I'm trying Arch Linux

_October 16th, 2025_

I'm love Apple products, but I'm getting frustrated with them.
I think iOS 26 is a blunder and have been holding off updating my laptop to any sort of liquid glass theme.
[DHH](https://world.hey.com/dhh) (read: programmer jesus) is doing a [linux thing](https://omarchy.org) too and it inspired me to buy a [framework laptop](https://frame.work) and install [Arch](https://archlinux.org) on it.

It's, uh, low level and tedious, especially in the beginning. You're writing to things that are read by the firmware of your system. I wrote an operating system in high school, so I was pretty sure I could swing this. As I installed it after work today, I asked ChatGPT tons of questions about what I was doing and what I remembered from my experience writing an OS. One memory address 0x7C00 is burned in my brain from 20 years ago. ChatGPT let me know that I'm old repeatedly and that things have changed.

As I went through the installation, I took notes. I got frustrated at one point, but these will be useful to me later.

---

## Arch Setup Notes

- `iwctl` launches `iwd`'s (**iNet wireless daemon**) "control utility"
  - Runs in the background and handles low level wireless stuff like scanning for networks, authenticating, and managing connections.
- "mounting" a drive "attaches" it to the current file system tree that starts at root /, somewhere in the tree to be accessed. Other drives can be formatted differently. Mounting tells the OS to make the stuff on that drive, in that drives file system format to be visible to us in our file tree, as if it were in our file system format
- `rfkill` radio frequency kill - shows all devices handed to the OS capable of emitting radio frequencies and allows toggling them off with hardware or software
- `cat /sys/class/power_supply/BAT1/capacity` to see battery level
  - Checking now while installing in the USB-OS of arch, I have 99; apparently it intentionally stops there because going to 100 can stress the chemistry of the battery.
  - Once arch is fully installed, it allows setting at the firmware level (hardware toggle) the max charge point: `fw-ectool --set-charge-limit 80`
- `fdisk -l` to list "block" devices (unformatted drives; without a filesystem)
- `lsblk` shows disk partitions and their mountpoints (if any) on the current filesystem

For idiots on a blank SSD with a bootable arch installer:

1. connect to the internet with iwctl
2. create two paritions 512MB and the remaining space (type +512M...) formatted as FAT32 and EXT4 respectively
3. mount the main parition (second, larger one) to the usb tree at /mnt
4. mount the bootloader (smaller, first, FAT32 partition) to the usb tree at /mnt/boot (which didn't exist for me, so mkdir it)
5. `pacstrap /mnt base linux linux-firmware`

- ESP = EFI System Partition = Bootloader Partition

6. Run this: `genfstab -U /mnt >> /mnt/etc/fstab` and believe you, me, I just had a 2 hour long conversation with ChatGPT about what exactly this is doing and why. I'm not sure I totally understand it. Things have changed since I was into this lower level stuff. Once our linux install is running with it's initial ram filesystem (initramfs), it needs to know which

Ok, fuck me, I guess. Kernels live _in_ the bootloader's partition now, on disk. That was the missing bit of understanding for me. If we're just gonna ball out with 512MB bootloaders, can we stop naming shit initramfs and just call it initial_ram_filesystem? I need someone to talk to other than the dog.

The bootloader has the kernel on disk. It loads it into RAM and is given a temporary in memory filesystem which has a script at `/init` in it that it runs which mounts the disk partition we intend to use for linux, and then it uses that partition's root as our new root (/). Then the kernel (or that script) calls the script at new root's `/sbin/init` location and runs that. On Arch Linux, that init script is called `systemd`. It does other stuff I guess.

7. `arch-chroot /mnt` to switch the current root directory to what it will be after restarts
8. `bootctl install` to move the bootloader binary into the boot directory... I don't know why the pacstrap command didn't do that though...
9. `ln -sf /usr/share/zoneinfo/US/Eastern /etc/localtime` archwiki says that once it has internet, there's a daemon running that tells the clock. I checked. It's true. But obviously at that point it assumes UTC. I assume after this, it adjusts for us.
10. `hwclock --systohc` overwrite the internal system coin-cell-powered clock with the system clock (which we just adjusted for us)
11. `locale-gen` then `pacman -Sy neovim` then edit `/etc/local.info` to have `LANG=en_US.UTF-8`
12. Identify your machine on networks by entering the name in plain text into `/etc/hostname` (for me, `Arsenic Arch` or `Lithium Linux`)

This is the best graphic ChatGPT has output (given my old understanding of the BIOS/MBR boot loader process from 20 years ago):

![](Screenshot%202025-10-16%20at%201.07.45%E2%80%AFAM.png)

13. Configure the boot loader `systemd-boot`:
    1. `mkdir -p /boot/loader/entries`
    2. Copy ... (take a picture I guess) the UUID in `blkid /dev/nvme0n1p2` (the ultimate root partition / drive)
    3. Paste this into `/boot/loader/entries/arch.conf`:
       (NOTE NOW: the space count between k/v's here don't matter; just needs whitespace)
       ```
         title   Arch Linux
         linux   /vmlinuz-linux
         initrd  /initramfs-linux.img
         options root=UUID=<your-UUID-or-PARTUUID> rw
       ```
    4. Optional, apparently, but good practice for the boot loader, create a `/boot/loader/loader.conf` with these contents:
       ```
       default  arch
       timeout  3
       editor   no
       ```
       That tells the boot loader to auto load the config in the `arch.conf` file after 3 seconds. `editor no` isn't selecting a default editor; I would have set nvim. It's controlling whether the boot loader lets you edit boot options at startup. Its options are either `yes` or `no`.
14. Run `bootctl list` to confirm our setup is there. `/boot//` will have double slashes. You can remove the leading `/` in arch.conf if you want, but know that it's okay that way.
    (NOTE: on reboot, I see a usbc "error" at my login prompt about USBC with an error code of `0`, which literally means "succes", confirmed by ChatGPT which also says that others have seen it and that when we upgrade the kernel, it will go away)
15. Login with `root` and the password we set earlier.

[All Posts](/README.md)
