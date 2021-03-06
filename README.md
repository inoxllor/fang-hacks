# Fang Hacks

Collection of modifications for the XiaoFang WiFi Camera

## General usage

Download an sd-card image from the [releases](https://github.com/samtap/fang-hacks/releases) page or follow the manual steps below.

### 1. Prepare an sd-card with two partitions.
The first partition on the device must be a vfat partition. It will only contain some small scripts so 100MiB should be more than enough.
The second partition must be an ext2 partition and will contain all other files.

### 2. Copy bootstrap folder and snx_autorun.sh
The bootstrap folder contains CGI scripts for the embedded Boa webserver. The ```snx_autorun.sh``` script is the entry-point for enabling the hacks.

Both must be copied to the vfat partition.

### 3. Copy data folder
The data folder must be copied to the ext2 partition.

### 4. Place sd-card in device
The device will automatically run ```snx_autorun.sh``` when the sd-card is inserted.

### 5. Enable hacks
When you visit ```http://device-ip/cgi-bin/status``` you should now be presented with a status page. If you get a '404 Not Found' page, the ```snx_autorun.sh``` script didn't run.

Please make sure to insert the sd-card after the device has finished booting. Sometimes the script isn't executed when the sd-card is already present during boot.

Click 'Apply' to enable the hacks.

## Background
The modifications aim to be as least intrusive as possible. Currently there's no recovery method when the device doesn't boot, so only a minimum of system files are modified. This means you can always revert to original behavior by simply removing the sd-card.

Two system modifications are made when you click Apply on the status page:

- A modified sdcard hotplug script is placed on the device to automatically mount ext2 volumes.
- A modified rc.local script is placed on the device to enable hacks when the device is rebooted. It also disables copying original files from /root/etc_default to prevent overwriting changes to the hotplug script.

Both are based on the original files and don't affect the original behaviour in any way.

## Hacks
When the status page shows the hacks have been applied successfully, the following features are available:
- You can place any binaries, scripts etc. you need in the data folder on the sd-card. The device only has limited space available on internal flash so you don't risk running out of space.
- A busybox build is provided with many applets available such as telnetd, ftpd, netcat.
- Scripts placed in data/etc/scripts will be automatically executed after the device boots.

# Features
By default the following features are enabled:
- FTP server to access and update files on sd-card
- RTSP server can be accessed on ```rtsp://device-ip/unicast```
- A telnet server (using the busybox from sd-card) is running on port 2323 (user: root, password: ismart12)
- A script controls the IR filter and LEDs
- Various features available via the status page ```http://device-ip/cgi-bin/status```
