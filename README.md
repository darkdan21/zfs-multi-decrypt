
# Custom ZFS multi decrypt

I recently built a NAS, with two encrypted ZFS pools, and wanted to be able to decrypt them on boot with the same password.
I couldn't find a way to do that, so here we are.
There are going to be problems with this, do don't use it - I've outlined some potential ones at the end.


## Install guide

1. Install Arch Linux with a ZFS encrypted root.
2.  Copy the `hooks/zfs_custom` and `install/zfs_custom` files into their respective directories in `/etc/initcpio`
3. Add `zfs_custom` into `/etc/mkinitcpio.conf` before the zfs hook (the zfs hook still does the mounting, and acts as a fallback if something breaks
4. Rebuild your initramfs: `sudo mkinitcpio -p linux` (or replace `linux` with your kernel of choice)
5. Reboot
6. Be sad because it doesn't work

### Potential problems

- If the ZFS commands change, this will probably break
- There are almost certainly problems with using the same passphrase on multiple ZFS pool keys, so if you really care about security don't do this - there are better solutions.

### Problems I had that I'm writing here so I remember why I did things a certain way

- I'm bad at shell scripting
- The shell that executes the script is `busybox sh`, so write it in that, not bash.
- I couldn't get the device to beep before and after the password prompt, which is sad.
- The build hook in install has to have `add_runscript` or nothing will happen.
