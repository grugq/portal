P.O.R.T.A.L.
======

Personal Onion Router To Assure Liberty

Overview
--------

PORTAL is a project that aims to keep people out of jail. It is a dedicated hardware device (a router) which forces all internet traffic to be sent over the Tor network. This significantly increases the odds of using Tor effectively, and reduces the potential to make fatal mistakes. 

On Hardware
------------
There are two ways to construct a PORTAL: using a chipped router (recommended), or using "the /overlay trick". Using a modified router (with sufficient onboard flash) provides the most flexibility for internet connectivity (either WiFi or 3G) as well as ensuring that the Tor process has sufficient RAM to run. A chipped router provides the best overall experience. The /overlay trick enables a stock router to union mount a microSD card for additional storage space. This allows for multiple microSD cards to provide different configurations (also possible with a chipped router), but has the downside of requiring a 3G modem with onboard microSD slot. 

### Supported Devices ###
#### Chipped Routers ####
* TP-LINK MR11U - 8mb flash storage, 64mb RAM
* TP-LINK WR703N - 8mb flash storage, 64mb RAM

#### Stock Routers ####
* TP-LINK MR11U - 4mb flash, 32mb RAM
* TP-LINK MR3040 - 4mb flash, 32mb RAM
* TP-LINK MR3020 - 4mb flash, 16mb RAM [WARNING: almost unusable]
* TP-LINK WR703N - 4mb flash, 32mb RAM

It is **highly recommended** to use a modified router. The modified MR11U and WR703N provide a better experience than the stock routers due to the additional RAM. The severe space constraints of the stock router make them very challenging to work with. Due to the lack of usable space, it is necessary to use an external disk to store the Tor packages. The stock router has only a single USB port, and the best option is to use a microSD in a 3G modem. 

PORTAL Constuction Guide: Modified Router
-----------------------------------------
### Requirements ###
* Router (modified)
* 3G modem or USB WiFi adapter
* (optional) microSD card (for extended storage on 3G modem)

### HOW TO ###
* step 0. Flash OpenWRT. If the router does not already have OpenWRT, then flash a stock OpenWRT build onto the router. Use the `Attitude Adjustment` development line. See the OpenWRT wiki for details. 
* step 1. Flash the PORTAL firmware image onto the router.
* step 2. Configure the PORTAL: set a root password, set a WPA2 password for the wifi, configure the internet uplink
* step 3. Done

PORTAL Construction Guide: Stock Router
---------------------------------------
### Requirements ###
* Router (see: Supported Devices)
* 3G modem w/ microSD slot (known working: ZTE MF680, Huawei E176, SierraWireless)
* microSD card (any size, faster is better)

### HOW TO ###
* step 0. Flash OpenWRT. Flash the `trunk` development line as it has a reduced footprint compared to the `Attitude Adjustment` line (no LuCi, etc.)
* step 1. Install the following packages:
    usb-modeswitch usb-modeswitch-data 
    kmod-usb-serial-option kmod-usb-serial-wwan 
    kmod-fs-ext4 kmod-scsi-core
    comgt chat
    block-mount
* step 2. Format the microSD card as an `ext4` filesystem. Make sure you pass the `-O "^has_journal"` option to `mkfs.ext4`.
    $ sudo mkfs.ext4 -O "^has_journal" /dev/$DEVICE
Insert the microSD into the 3G modem and the modem into the router.
* step 3. Configure the microSD card to appear as an overlay on the router. Edit `/etc/config/fstab` to contain a section for `/dev/sda1` with a mountpoint of `/overlay`, set to automatically mount. Reboot the router. See the [OpenWRT wiki](http://wiki.openwrt.org/doc/howto/extroot) for details, specifically the __New external overlay variant (pivot overlay)__ section.
* step 4. Install the following packages:
    tor-alpha tor-alpha-geoip
    ipt-mod-conntrack ipt-mod-conntrack-extras
* step 5. Configure Tor. For an example see [this OpenWRT forum post](https://forum.openwrt.org/viewtopic.php?id=27354). Most of the configuration options are included in the PORTAL `files/etc/` in this project.
    
