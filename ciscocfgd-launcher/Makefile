# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ciscocfgd-launcher
PKG_VERSION:=0.0.1
PKG_RELEASE:=1

PKG_MAINTAINER:=Empo Inc. <admin@empo.im>
PKG_LICENSE:=GPL-3
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/ciscocfgd-launcher
  SUBMENU:=Python
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Cisco configuration daemon initscript
  URL:=https://mareel.io
  DEPENDS:=+python3-ciscocfg
endef

define Build/Compile
	# Do nothing
endef

define Package/ciscocfgd-launcher/install
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/etc/init.d/ciscocfgd $(1)/etc/init.d/
endef

$(eval $(call BuildPackage,ciscocfgd-launcher))