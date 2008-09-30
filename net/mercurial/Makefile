#
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# $Id$

include $(TOPDIR)/rules.mk

PKG_NAME:=mercurial
PKG_VERSION:=1.0.2
PKG_RELEASE:=1

PKG_SOURCE_URL:=http://selenic.com/mercurial/release/
PKG_SOURCE:=mercurial-$(PKG_VERSION).tar.gz
PKG_MD5SUM:=32432616f517107e6582721c257cd1f4
PKG_BUILD_DEPENDS:=python

include $(INCLUDE_DIR)/package.mk

define Package/mercurial
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=python
  TITLE:=Mercurial Source Control Management (SCM) system
  URL:=http://www.selenic.com/mercurial/
endef

define Package/mercurial/description
  A fast, lightweight Source Control Management system designed for efficient handling of very large distributed projects.
endef

HG_BUILDOPTS= \
	CC="$(TARGET_CC)" \
	LDSHARED="$(TARGET_CC) -shared" \
	CFLAGS="$(TARGET_CFLAGS) -I$(STAGING_DIR)/usr/include/python2.5/" \
	LDFLAGS="-L$(STAGING_DIR)/usr/lib -lpython2.5"

define Build/Compile
	(cd $(PKG_BUILD_DIR); \
		$(HG_BUILDOPTS) /usr/bin/python setup.py build; \
	);
	(cd $(PKG_BUILD_DIR); \
		/usr/bin/python setup.py install --prefix="$(PKG_INSTALL_DIR)"; \
	);
endef

define Package/mercurial/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/bin $(1)/usr
	$(CP) $(PKG_INSTALL_DIR)/lib $(1)/usr
endef

$(eval $(call BuildPackage,mercurial))