#
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=gettext
PKG_VERSION:=0.16.1
PKG_RELEASE:=4

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=@GNU/gettext
PKG_MD5SUM:=3d9ad24301c6d6b17ec30704a13fe127

PKG_FIXUP = libtool

include $(INCLUDE_DIR)/package.mk

define Package/libintl
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=GNU Internationalization library
  URL:=http://www.gnu.org/software/gettext/
endef

TARGET_CFLAGS += $(FPIC)

CONFIGURE_ARGS += \
	--enable-shared \
	--enable-static \
	--disable-rpath \
	--enable-nls \
	--disable-java \
	--disable-native-java \
	--disable-openmp \
	--with-included-gettext \
	--without-libintl-prefix \
	--without-libexpat-prefix \
	--without-emacs

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) DESTDIR="$(PKG_INSTALL_DIR)" all
endef

define Build/Install
	$(MAKE) -C $(PKG_BUILD_DIR) DESTDIR="$(PKG_INSTALL_DIR)" install
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/lib/libintl/include
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/include/libintl.h $(1)/usr/lib/libintl/include/
	$(INSTALL_DIR) $(1)/usr/lib/libintl/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libintl.{a,so*} $(1)/usr/lib/libintl/lib/
endef

define Package/libintl/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libintl.so.* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libintl))
