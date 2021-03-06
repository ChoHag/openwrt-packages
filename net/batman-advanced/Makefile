#
# Copyright (C) 2008 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# $Id: Makefile 5624 2006-11-23 00:29:07Z nbd $

include $(TOPDIR)/rules.mk

PKG_NAME:=batman-adv
PKG_REV:=1545
PKG_VERSION:=r$(PKG_REV)
PKG_RELEASE:=1

PKG_KMOD_VERSION=caf7148e887b57abf37ef89a1b5aedf314893c94
PKG_KMOD_FNAME=batman-adv-maint-$(PKG_KMOD_VERSION)
PKG_KMOD_BUILD_DIR:=$(BUILD_DIR)/batman-adv

PKG_SOURCE_PROTO:=svn
PKG_SOURCE_VERSION:=$(PKG_REV)
PKG_SOURCE_SUBDIR:=batctl-0.2.x-$(PKG_VERSION)
PKG_SOURCE_URL:=http://downloads.open-mesh.net/svn/batman/branches/batctl-0.2.x/
PKG_SOURCE:=$(PKG_SOURCE_SUBDIR).tar.gz
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_SOURCE_SUBDIR)

PKG_TOOL_INSTALL_DIR:=$(PKG_BUILD_DIR)/ipkg-batctl-install
PKG_TOOL_BUILD_DIR:=$(PKG_BUILD_DIR)

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/kernel.mk

define Package/batman-adv/Default
  URL:=http://www.open-mesh.net/
  MAINTAINER:=Marek Lindner <lindner_marek@yahoo.de>
endef

define KernelPackage/batman-adv-kernelland
$(call Package/batman-adv/Default)
  SUBMENU:=Network Support
  DEPENDS:=@LINUX_2_6
  TITLE:=B.A.T.M.A.N. layer 2
  FILES:=$(PKG_KMOD_BUILD_DIR)/batman-adv.$(LINUX_KMOD_SUFFIX)
  AUTOLOAD:=$(call AutoLoad,50,batman-adv)
endef

define KernelPackage/batman-adv-kernelland/config
	source "$(SOURCE)/Config.in"
endef

define KernelPackage/batman-adv-kernelland/description
B.A.T.M.A.N. layer 2
endef

define Download/batman-adv-maint
  FILE:=$(PKG_KMOD_FNAME)
  URL:=http://git.open-mesh.org/snapshot
endef
$(eval $(call Download,batman-adv-maint))

MAKE_BATADV_KERNELLAND_ARGS += \
	CROSS_COMPILE="$(TARGET_CROSS)" \
	ARCH="$(LINUX_KARCH)" \
	PATH="$(TARGET_PATH)" \
	$(if $(CONFIG_KMOD_BATMAN_ADV_DEBUG_LOG),EXTRA_FLAGS="-DCONFIG_BATMAN_ADV_DEBUG") \
	SUBDIRS="$(PKG_KMOD_BUILD_DIR)" \
	LINUX_VERSION="$(LINUX_VERSION)" \
	REVISION="$(PKG_REV)" modules

MAKE_BATCTL_ARGS += \
	CFLAGS="$(TARGET_CFLAGS)" \
	CCFLAGS="$(TARGET_CFLAGS)" \
	OFLAGS="$(TARGET_CFLAGS)" \
	REVISION="$(PKG_REV)" \
	CC="$(TARGET_CC)" \
	NODEBUG=1 \
	UNAME="Linux" \
	INSTALL_PREFIX="$(PKG_INSTALL_DIR)" \
	STRIP="/bin/true" \
	batctl install

define Build/Configure
endef

ifneq ($(DEVELOPER)$(CONFIG_PACKAGE_kmod-batman-adv-kernelland),)
        BUILD_KMOD_ADV = $(MAKE) -C "$(LINUX_DIR)" $(MAKE_BATADV_KERNELLAND_ARGS)
endif

ifneq ($(DEVELOPER)$(CONFIG_PACKAGE_kmod-batman-adv-kernelland),)
        BUILD_BATCTL = $(MAKE) -C $(PKG_TOOL_BUILD_DIR) $(MAKE_BATCTL_ARGS)
endif

define Build/Compile
	tar xzf "$(DL_DIR)/$(PKG_KMOD_FNAME)" -C "$(BUILD_DIR)"
	cp $(PKG_KMOD_BUILD_DIR)/Makefile.kbuild $(PKG_KMOD_BUILD_DIR)/Makefile
	$(BUILD_KMOD_ADV)
	$(BUILD_BATCTL)
endef

define Build/Clean
        rm -rf $(PKG_KMOD_BUILD_DIR)
endef

define KernelPackage/batman-adv-kernelland/install
	$(INSTALL_DIR) $(1)/etc/config $(1)/etc/init.d $(1)/usr/sbin
	$(INSTALL_BIN) ./files/etc/init.d/batman-adv-kernelland $(1)/etc/init.d
	$(INSTALL_DATA) ./files/etc/config/batman-adv-kernelland $(1)/etc/config
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/batctl $(1)/usr/sbin/
endef

$(eval $(call KernelPackage,batman-adv-kernelland))
