#
# Copyright (C) 2006-2009 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=clearsilver
PKG_VERSION:=0.10.5
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://www.clearsilver.net/downloads/
PKG_MD5SUM:=b8c0c7fbe0ef5e06e0c935f134304d44

include $(INCLUDE_DIR)/package.mk

define Package/clearsilver
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=ClearSilver template system
  URL:=http://www.clearsilver.net/
  MENU:=1
endef

define Package/clearsilver/description
  Clearsilver is a fast, powerful, and language-neutral HTML template system. In both static content sites and dynamic HTML applications, it provides a separation between presentation code and application logic which makes working with your project easier.
endef

TARGET_CFLAGS += $(FPIC)

CONFIGURE_ARGS += \
	--disable-wdb \
	--disable-apache \
	--disable-python \
	--disable-perl \
	--disable-ruby \
	--disable-java \
	--disable-csharp \
	--prefix=/usr

ifneq ($(SDK)$(CONFIG_CLEARSILVER_ENABLE_COMPRESSION),y)
  CONFIGURE_ARGS+= \
        --disable-compression
else
  CONFIGURE_ARGS+= \
        --enable-compression
endif

ifneq ($(SDK)$(CONFIG_CLEARSILVER_ENABLE_REMOTE_DEBUGGER),y)
  CONFIGURE_ARGS+= \
        --disable-remote-debugger
else
  CONFIGURE_ARGS+= \
        --enable-remote-debugger
endif

ifneq ($(SDK)$(CONFIG_CLEARSILVER_ENABLE_GETTEXT),y)
  CONFIGURE_ARGS+= \
        --disable-gettext
else
  CONFIGURE_ARGS+= \
        --enable-gettext
endif

MAKE_FLAGS += \
	$(TARGET_CONFIGURE_OPTS) \
	AR="$(AR) cr" \
	LD="$(TARGET_CC) -o" \
	DESTDIR="$(PKG_INSTALL_DIR)" \
	all install

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/ClearSilver $(1)/usr/include/

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libneo_*.a $(1)/usr/lib/

	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin/
endef

define Package/clearsilver/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/libs/libneo_*.a $(1)/usr/lib/
endef

$(eval $(call BuildPackage,clearsilver))
