# HOW TO INSTALL P0RTAL
## Updated for new GL.iNet devices

The `files/` directory is the root of the GL.iNet device. There is an `etc/`
directory that contains all the configuration files which will collectively
setup a stock OpenWRT GL.iNet device as a p0rtal. 

# Requirements

1. GL.iNet: MiFi
2. Stock OpenWRT
3. Install the `tor` package on the device
4. Copy the files in `files/etc/` into the router's `/etc/` directory
5. Tweak the configurations as necessary, in particular:
	- the `wireless` configuration might need adjustment
	- the `/etc/firewall.user` script may have unnecessary glnet features
	- on the MiFi the `wwan` might not be in the correct firewall groups

# p0rtal Usage Guide

1. This configuration of the p0rtal uses normal naming for OpenWRT:
	- `lan` --> the TOR sandbox (172.16.8.0/24)
	    + :9050 --> Tor SOCKS5 port, open proxy
	    + :9040 --> transparent TCP
	    + :9053 --> transparent DNS
	- `wan` --> the ADMIN interface (10.10.10.0/24)
	- `wwan` --> the INTERNET
2. This configuration has not been hardened or stripped. It is for testing.
