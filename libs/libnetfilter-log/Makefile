#
# Copyright (C) 2009 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=libnetfilter_log
PKG_VERSION:=0.0.16
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=ftp://ftp.netfilter.org/pub/libnetfilter_log/ \
	http://mirrors.evolva.ro/netfilter.org/libnetfilter_log/
PKG_MD5SUM:=4ffcf46dad9d0c81f6611dd185639694

PKG_FIXUP:=libtool

include $(INCLUDE_DIR)/package.mk

define Package/libnetfilter-log
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=@LINUX_2_6 +libnfnetlink +kmod-nfnetlink-log
  TITLE:=API to packets logged by the kernel packet filter
  URL:=http://www.netfilter.org/projects/libnetfilter_log/
endef

define Package/libnetfilter-log/description
 libnetfilter_log is a userspace library providing interface to
 packets that have been logged by the kernel packet filter.
endef

TARGET_CFLAGS += $(FPIC)

CONFIGURE_ARGS += \
	--enable-static \
	--enable-shared \

CONFIGURE_VARS += \
	LIBNFNETLINK_CFLAGS="$(TARGET_CPPFLAGS)" \
	LIBNFNETLINK_LIBS="$(TARGET_LDFLAGS)"

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) all
	$(call libtool_fixup_libdir)
	$(MAKE) -C $(PKG_BUILD_DIR) DESTDIR="$(PKG_INSTALL_DIR)" install
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/libnetfilter_log
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/include/libnetfilter_log/*.h \
		$(1)/usr/include/libnetfilter_log/

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/libnetfilter_log*.{so*,a,la} \
		$(1)/usr/lib/

	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/lib/pkgconfig/libnetfilter_log.pc \
		$(1)/usr/lib/pkgconfig/
endef

define Package/libnetfilter-log/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libnetfilter_log*.so.* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libnetfilter-log))
