#
# Copyright (C) 2006-2008 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=dhcp
PKG_VERSION:=4.1.0p1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=ftp://ftp.isc.org/isc/dhcp/
PKG_MD5SUM:=325ff8338c5a21f89d5304ac13ffebdf

include $(INCLUDE_DIR)/package.mk

define Package/dhcp/Default
  SECTION:=net
  CATEGORY:=Network
  TITLE:=ISC's DHCP
  URL:=https://www.isc.org/software/dhcp
endef

define Package/dhcp-client
  $(call Package/dhcp/Default)
  TITLE+= client
endef

define Package/dhcp-relay
  $(call Package/dhcp/Default)
  TITLE+= relay
endef

define Package/dhcp-server
  $(call Package/dhcp/Default)
  TITLE+= server
endef

CONFIGULE_ARGS += \
	--localstatedir=/var \
	--with-srv-lease-file=/etc/dhcpd.leases \
	--with-srv6-lease-file=/etc/dhcpd6.leases

CONFIGURE_VARS += ac_cv_file__dev_random=set

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		CFLAGS=-D_GNU_SOURCE \
		all install
endef


define Package/dhcp-client/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dhclient $(1)/usr/sbin/
endef

define Package/dhcp-relay/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dhcrelay $(1)/usr/sbin/
endef

define Package/dhcp-server/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/dhcpd $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/omshell $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/dhcpd.init $(1)/etc/init.d/dhcpd
	$(INSTALL_BIN) ./files/dhcpd6.init $(1)/etc/init.d/dhcpd6
endef

$(eval $(call BuildPackage,dhcp-client))
$(eval $(call BuildPackage,dhcp-relay))
$(eval $(call BuildPackage,dhcp-server))
