# ROCK Pi RAID automount setup
Unfortunately, the ROCK PI Dual/Quad SATA HAT board is initialized and started after all system relevant actions and services are already run. Therefore the usually way of mounting drives via /etc/fstab is not possible (because the drives cannot be detected at that stage).

Fortunately, we can setup a custom systemd mount to automount our RAID.
I assume you already setup a single linux software raid with mdadm (/dev/md0/) and created a ext4 filesystem with mkfs.ext4. 

Installation:
1. Copy the files raid.mount and raid.automount to your Raspberry PI
2. (Optional) Replace the `Where` path in both files to the location you want to mount your RAID in
3. Create the directory set in the `Where` path e.g. `mkdir -p /mnt/md0`
4. Move both files to /etc/system/system/
5. Register the services `sudo systemctl enable raid.mount`
6. Reboot
