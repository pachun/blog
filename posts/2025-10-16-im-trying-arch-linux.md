# I'm trying Arch Linux

_October 16th, 2025_

I've always loved Apple products.
I've been on Apple's side through a bunch of controversial periods.
Should they really remove the CD rom drive from computers? Yep.
Should they really remove the headphone jack from iPhones? Yep.

Recently I've been falling out of love, though.
I think iOS 26 and Liquid Glass is a blunder.
Bugs and legibility aside, it just looks bad.

The guy who largely made Ruby on Rails ([DHH](https://world.hey.com/dhh)) is also falling out of love with Apple and looking for alternatives, (though I think for [different reasons](https://world.hey.com/dhh/hey-is-finally-for-sale-on-the-iphone-a08cb218)).
Reading about the new [linux distribution](https://omarchy.org) he recently released and how [successful it's been]() inspired me to try linux again.

I haven't been back to linux since college and I did write an operating system in high school for fun, so I was mostly confident I could swing a new Arch installation.

So I bought a [framework laptop](https://frame.work) and installed [Arch](https://archlinux.org) on it.
It's only been a day - but it's been blast so far.

As in installed Arch after work last night, I took notes.
I did get frustrated at several points, but ChatGPT held me hand through the whole thing.
I'm leaving those notes here for future me and the six of you who've ever read this blog.

There are points where I swear a bit. I think it adds authenticity.

---

## Arch Setup Notes

- `iwctl` launches `iwd`'s (**iNet wireless daemon**) _control utility_
  - Runs in the background and handles low level wireless stuff like scanning for networks, authenticating, and managing connections.
- _mounting_ a drive "attaches" it to the current file system tree that starts at root `/`, somewhere in the tree to be accessed. Other drives can have different file system formats. Mounting tells the OS to make the stuff on the differently formatted, mounted drive, visible to us in our file tree, as if it were a part of our file system.
- `rfkill` radio frequency kill - shows all devices capable of emitting radio frequencies and allows toggling them off with hardware or software.
- `cat /sys/class/power_supply/BAT1/capacity` to see battery level
  - Checking now while installing in the USB-OS of arch, I have %99; apparently it intentionally stops there because going to %100 can stress the chemistry of the battery.
  - Once arch is fully installed, it allows setting the max charge percentage at the firmware level: `fw-ectool --set-charge-limit 80`
- `fdisk -l` to list "block" devices (unformatted drives; without a filesystem)
- `lsblk` shows disk partitions and their mountpoints (if any) on the current filesystem

Installing Arch:

1. Write the arch .iso image to a USB flash drive with `dd` on your mac.
1. Connect to the internet with iwctl
1. Create two paritions
   - 512MB (type +512M in `fdisk` - don't forget the `+`) as FAT32 for the UEFI
   - Use the remaining space formatted as EXT4 for the root filesystem partition
1. Mount the main parition (the second, larger one) to the usb tree at /mnt
1. Mount the bootloader (smaller, earlier, FAT32 partition) to the usb tree at /mnt/boot (which didn't exist for me, so `mkdir` it)
1. Install Arch: `pacstrap /mnt base linux linux-firmware`
   - ESP = EFI System Partition = Bootloader Partition
1. Run this: `genfstab -U /mnt >> /mnt/etc/fstab` and believe you, me, I just had a 2 hour long conversation with ChatGPT about what exactly this is doing and why. I'm not sure I totally understand it. Things have changed since I was into this lower level stuff. Once our linux install is running with it's initial ram filesystem (initramfs), it needs to know which

Wowzers. Breakthrough moment. Kernels live _in_ the bootloader's partition now, on disk. Not in the last, largest OS filesystem partition. That was the missing bit of understanding in my case. If we're just gonna ball out with 512MB bootloaders, can we stop naming shit initramfs and call it initial_ram_filesystem? I need someone to talk to other than the dog.

The bootloader has the kernel on disk, in its partition. It loads it into RAM and is given a temporary in memory filesystem which has a script at `/init` in it that it runs which mounts the disk partition we intend to use for linux, and then it uses that partition's root as our new root (`/`). Then the kernel (or that `init` script?) calls the script at new root's `/sbin/init` location and runs that. On Arch Linux, that `/sbin/init` script is called `systemd`. Base on my conversations with ChatGPT, it does other stuff too, I guess...

1. `arch-chroot /mnt` to switch the current root directory to what it will be after restarts.
1. `bootctl install` to move the bootloader binary into the boot directory... I don't know why the pacstrap command didn't do that though...
1. `ln -sf /usr/share/zoneinfo/US/Eastern /etc/localtime` archwiki says that once it has internet, there's a daemon running that tells the clock. I checked. It's true. But obviously at that point it assumes UTC. After this, it adjusts for our TZ.
1. `hwclock --systohc` overwrite the internal system coin-cell-powered clock with the system clock (which we just adjusted for us)
1. `locale-gen` then `pacman -Sy neovim` then edit `/etc/local.info` to have `LANG=en_US.UTF-8`
1. Identify your machine on networks by entering a name for our machine in plain text in `/etc/hostname` (for me, `Arsenic Arch` or `Lithium Linux`)

This is the best graphic ChatGPT has output (given my old understanding of the BIOS/MBR boot loader process from 20 years ago):

<p align="center">
  <img src="/posts/assets/2025-10-16-im-trying-arch-linux/photo.png" height="600" alt="photo" />
</p>

1. Configure the boot loader `systemd-boot`:
   1. `mkdir -p /boot/loader/entries`
   2. Copy ... (take a picture of it, I guess) the UUID in `blkid /dev/nvme0n1p2` (the ultimate root partition / drive)
   3. Paste this into `/boot/loader/entries/arch.conf`:
      (NOTE NOW: the space count between keys and values here doesn't matter; it just needs any amount of whitespace)
      ```
        title   Arch Linux
        linux   /vmlinuz-linux
        initrd  /initramfs-linux.img
        options root=UUID=<your-UUID-or-PARTUUID> rw
      ```
   4. Optional, apparently, but good practice for the boot loader, create a `/boot/loader/loader.conf` with these contents:
      ```
      default  arch
      timeout  0
      editor   no
      ```
      That tells the boot loader to auto load the config in the `arch.conf` file immediately (after 0 seconds). `editor no` isn't selecting a default editor; I would have set nvim. It's controlling whether the boot loader lets you edit boot options at startup. Its options are either `yes` or `no`.
2. Run `bootctl list` to confirm our setup is there.

- `/boot//` will have double slashes. You can remove the leading `/` in `arch.conf` if you want, but know that it's okay this way.

3. `reboot` - We should do this to get a version of the kernel that only has what we specifically installed when we installed arch by running `pacstrap /mnt base linux linux-firmware` - We could have specified other things there, and the bootable USB actually _does_ have things in it that the plain and basic arch install we chose does not have; If we get too used to the tooling available in the bootable USB environment, it's a little misleading for where we wind up after reboot. So reboot.

- (NOTE: on reboot, I see a usbc "error" at my login prompt about USBC with an error code of `0`, which literally means "succes", confirmed by ChatGPT which also says that others have seen it and that when we upgrade the kernel, it will go away)

4. Login with `root` and the password we set earlier.

[All Posts](/README.md)
