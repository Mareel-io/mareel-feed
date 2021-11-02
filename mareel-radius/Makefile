# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=mareel-radius
PKG_VERSION:=0.0.1
PKG_RELEASE:=1

PKG_MAINTAINER:=Empo Inc. <admin@empo.im>
# TODO: FIXME
PKG_LICENSE:=UNLICENSED
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/mareel-radius
  CATEGORY:=Utilities
  TITLE:=mareel-radius
  URL:=https://mareel.io
  DEPENDS:=+xz-utils +zstd +gnupg2 +abtool
endef

define Package/mareel-radius/description
  Mareel (lightweight) RADIUS server
endef

define Build/Compile
  # Do nothing
endef

define Package/mareel-radius/install
	$(INSTALL_DIR) $(1)/usr
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) ./files/sysupgrade/mareel-sysupgrade $(1)/usr/sbin/
	$(INSTALL_BIN) ./files/sysupgrade/mareel-restore $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/etc/init.d/mareel-restore $(1)/etc/init.d/mareel-restore
endef

$(eval $(call BuildPackage,mareel-radius))
