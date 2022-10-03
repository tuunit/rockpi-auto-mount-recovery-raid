# ROCK Pi RAID auto-mount or auto-recovery setup
Unfortunately, the ROCK PI Dual/Quad SATA HAT board is initialized and started after all system relevant actions and services are already run. Therefore the usually way of mounting drives via /etc/fstab is not possible (because the drives cannot be detected at that stage).

Fortunately, we can setup a custom systemd mount to automount our RAID.
I assume you already setup a single linux software raid with mdadm (/dev/md0/) and created a ext4 filesystem with mkfs.ext4. 

## Auto-mount vs Auto-recovery

Furthermore, I experienced a lot of instability with the software raid. Which is why a simple automount solution wasn't working for me. Therefore you will find two solutions in this repository, a clean/stable solution in the `auto-mount` directory for just automounting a stable RAID. And for the paranoid, as I'm, a less optimal solution for triggering a recovery and mount after every boot in the `auto-recovery` directory. 

## Installation guide for the stable _automount_ solution:

1. Copy the files raid.mount and raid.automount from the `auto-mount` directory to your Raspberry PI
2. (Optional) Replace the `Where` path in both files to the location you want to mount your RAID in
3. Create the directory set in the `Where` path e.g. `mkdir -p /mnt/md0`
4. Move both files to `/etc/system/system/`
5. Register the automount `sudo systemctl enable raid.mount`
6. Reboot

## Installation guide for the _autorecovery_ solution:

> Note: This implementation is not super clean and should be used with extra caution. Please take your time to check the code for yourself.
1. Copy the files `auto_raid_recovery.service` and `auto_raid_recovery` from the `auto-recovery` to your Raspberry PI
2. (Optional) Change the mount path in line 30 in `auto_raid_recovery` to the location you want to mount your RAID in 
3. Create the directory mount path e.g. `mkdir -p /mnt/md0`
4. Add execution flag to `auto_raid_recovery` with `chmod +x auto_raid_recovery` and move to `/usr/local/bin/`
5. Move `auto_raid_recovery.serivce` to `/etc/system/system/`
6. Register the service `sudo systemctl enable auto_raid_recovery.service`
7. Reboot
