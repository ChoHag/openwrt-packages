#
# Copyright (C) 2006-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=stress
PKG_VERSION:=1.0.2
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://weather.ou.edu/~apw/projects/stress/
PKG_MD5SUM:=ad53eb802150bda8f898be9ebf3b4b36

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/stress
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS:=@LINUX_2_6
  TITLE:=stress is a simple stress utility
  URL:=http://weather.ou.edu/~apw/projects/stress/
endef

define Package/stress/description
	stress is a simple tool that imposes certain types of compute \	stress
	on UNIX-like operating systems.
endef

CONFIGURE_ARGS += \
	--prefix="/usr"

MAKE_FLAGS += \
	CFLAGS="$(TARGET_CFLAGS)"

define Package/stress/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/stress $(1)/usr/bin
endef

$(eval $(call BuildPackage,stress))
