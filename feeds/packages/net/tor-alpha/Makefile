#
# Copyright (C) 2008-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=tor-alpha
PKG_VERSION:=0.2.3.22-rc
PKG_RELEASE:=1

PKG_SOURCE:=tor-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://www.torproject.org/dist \
	https://archive.torproject.org/tor-package-archive
PKG_MD5SUM:=c07431ee40a0e16dc1b4d1e8d266680c

PKG_BUILD_DEPENDS:=libminiupnpc libnatpmp
PKG_BUILD_DIR:=$(BUILD_DIR)/tor-$(PKG_VERSION)
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/tor-alpha/Default
  SECTION:=net
  CATEGORY:=Network
  URL:=https://www.torproject.org/
endef

define Package/tor-alpha/Default/description
 Tor is a toolset for a wide range of organizations and people that want to
 improve their safety and security on the Internet. Using Tor can help you
 anonymize web browsing and publishing, instant messaging, IRC, SSH, and
 more. Tor also provides a platform on which software developers can build
 new applications with built-in anonymity, safety, and privacy features.
endef

define Package/tor-alpha
$(call Package/tor-alpha/Default)
  TITLE:=An anonymous Internet communication system
  DEPENDS:=+libevent2 +libevent2-openssl +libpthread +librt
endef

define Package/tor-alpha/description
$(call Package/tor-alpha/Default/description)
 This package contains the tor daemon.
endef

define Package/tor-alpha-fw-helper
$(call Package/tor-alpha/Default)
  TITLE:=Firewall helper for tor
  DEPENDS:=+tor-alpha +libminiupnpc +libnatpmp
endef

define Package/tor-alpha-fw-helper/description
$(call Package/tor-alpha/Default/description)
 This package contains a helper for automatically configuring port forwarding.
endef

define Package/tor-alpha-geoip
$(call Package/tor-alpha/Default)
  TITLE:=GeoIP db for tor
  DEPENDS:=+tor-alpha
endef

define Package/tor-alpha-geoip/description
$(call Package/tor-alpha/Default/description)
 This package contains a GeoIP database mapping IP addresses to countries.
endef

define Package/tor-alpha/conffiles
/etc/tor/torrc
endef

CONFIGURE_ARGS += \
	--with-libevent-dir="$(STAGING_DIR)/usr" \
	--with-ssl-dir="$(STAGING_DIR)/usr" \
	--enable-upnp \
	--with-libminiupnpc-dir="$(STAGING_DIR)/usr" \
	--enable-nat-pmp \
	--with-libnatpmp-dir="$(STAGING_DIR)/usr" \
	--disable-asciidoc

ifneq ($(CONFIG_SSP_SUPPORT),y)
	CONFIGURE_ARGS += \
		--disable-gcc-hardening
endif

CONFIGURE_VARS += \
	CROSS_COMPILE="yes"

# pass CFLAGS again to override -O2 set by configure
MAKE_FLAGS += \
	CFLAGS="$(TARGET_CFLAGS)"

define Package/tor-alpha/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/tor $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/tor.init $(1)/etc/init.d/tor
	$(INSTALL_DIR) $(1)/etc/tor
	$(INSTALL_CONF) $(PKG_INSTALL_DIR)/etc/tor/torrc.sample $(1)/etc/tor/torrc
endef

define Package/tor-alpha-fw-helper/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/tor-fw-helper $(1)/usr/bin/
endef

define Package/tor-alpha-geoip/install
	$(INSTALL_DIR) $(1)/usr/share/tor
	$(CP) $(PKG_INSTALL_DIR)/usr/share/tor/geoip $(1)/usr/share/tor/
endef

$(eval $(call BuildPackage,tor-alpha))
$(eval $(call BuildPackage,tor-alpha-fw-helper))
$(eval $(call BuildPackage,tor-alpha-geoip))
