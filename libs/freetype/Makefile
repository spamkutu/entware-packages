#
# Copyright (C) 2006-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=freetype
PKG_VERSION:=2.7.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=@SF/freetype
PKG_HASH:=3a3bb2c4e15ffb433f2032f50a5b5a92558206822e22bfe8cbe339af4aa82f88

PKG_LICENSE:=FTL GPL-2.0 MIT ZLIB
PKG_LICENSE_FILES:=docs/LICENSE.TXT docs/FTL.TXT docs/GPLv2.TXT src/bdf/README src/pcf/README src/gzip/zlib.h
PKG_MAINTAINER:=Nicolas Thill <nico@openwrt.org>

PKG_FIXUP:=autoreconf
PKG_LIBTOOL_PATHS:=builds/unix

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

define Package/libfreetype
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=A free, high-quality and portable font engine
  URL:=http://www.freetype.org/
  DEPENDS:=+zlib +libbz2
endef

define Package/libfreetype/description
 The FreeType project is a team of volunteers who develop free,
 portable and high-quality software solutions for digital typography.
 They specifically target embedded systems and focus on bringing small,
 efficient and ubiquitous products.
endef

TARGET_CFLAGS += $(FPIC)

CONFIGURE_ARGS += \
	--enable-shared \
	--enable-static \
	--with-bzip2=yes \
	--with-zlib=yes \
	--with-png=no \

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) DESTDIR="$(PKG_INSTALL_DIR)" all install
endef

define Build/InstallDev
	$(INSTALL_DIR) $(2)/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/freetype-config $(2)/bin/
	$(INSTALL_DIR) $(1)/opt/include
	$(CP) $(PKG_INSTALL_DIR)/opt/include/freetype2 $(1)/opt/include/
	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/libfreetype.{a,so*} $(1)/opt/lib/
	$(INSTALL_DIR) $(1)/opt/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/pkgconfig/freetype2.pc $(1)/opt/lib/pkgconfig/

	$(SED) \
		's,^\(prefix\|exec_prefix\)=.*,\1="$(STAGING_DIR)/opt",; \
		 s,^\(includedir\)=.*,\1="$(STAGING_DIR)/opt/include",; \
		 s,^\(libdir\)=.*,\1="$(STAGING_DIR)/opt/lib",' \
		 $(2)/bin/freetype-config
	$(SED) \
		's,/opt/include,$$$${prefix}/include,g; \
		 s,/opt/lib,$$$${exec_prefix}/lib,g' \
		 $(1)/opt/lib/pkgconfig/freetype2.pc
endef


define Package/libfreetype/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/libfreetype.so.* $(1)/opt/lib/
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,libfreetype))
