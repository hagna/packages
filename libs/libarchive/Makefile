#
# Copyright (C) 2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libarchive
PKG_VERSION:=3.1.2
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://www.libarchive.org/downloads
PKG_MD5SUM:=efad5a503f66329bb9d2f4308b5de98a
PKG_MAINTAINER:=Johannes Morgenroth <morgenroth@ibr.cs.tu-bs.de>
PKG_LICENSE:=BSD-2-Clause

PKG_INSTALL:=1
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)/Default
	SECTION:=libs
	CATEGORY:=Libraries
	DEPENDS:=+zlib
	TITLE:=Multi-format archive and compression library
	URL:=http://www.libarchive.org/
endef

define Package/$(PKG_NAME)
	$(call Package/$(PKG_NAME)/Default)
	DEPENDS += +libopenssl
endef

define Package/$(PKG_NAME)-noopenssl
	$(call Package/$(PKG_NAME)/Default)
	TITLE += (without OpenSSL dependency)
	VARIANT:=noopenssl
endef

CONFIGURE_ARGS += \
	--disable-bsdcpio \
	--disable-bsdtar \
	--disable-acl \
	--disable-xattr \
	--without-bz2lib \
	--without-lzma \
	--without-lzmadec \
	--without-lzo2 \
	--without-nettle \
	--without-expat \
	--without-xml2

ifeq ($(BUILD_VARIANT),noopenssl)
	CONFIGURE_ARGS += --without-openssl
else
	CONFIGURE_ARGS += --with-openssl
endif

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/* $(1)/usr/lib/
endef

define Package/libarchive/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so.* $(1)/usr/lib/
endef
Package/$(PKG_NAME)-noopenssl/install = $(Package/$(PKG_NAME)/install)

$(eval $(call BuildPackage,libarchive))
$(eval $(call BuildPackage,libarchive-noopenssl))
