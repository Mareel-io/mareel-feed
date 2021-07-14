# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=python-ciscocfg
PKG_VERSION:=0.1
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://github.com/Mareel-io/ciscocfg.git
PKG_SOURCE_VERSION:=be9265e9f6cfa88727418e37a9b2f9ae6533b6ae
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_LICENSE:=GPL-3.0
PKG_LICENSE_FILES:=LICENSE

include ../pypi.mk
include $(INCLUDE_DIR)/package.mk
include ../python3-package.mk

define Package/python3-ciscocfg
	SUBMENU:=Python
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=Cisco Config parser
	URL:=https://mareel.io
	DEPENDS:=+python3-msgpack +python3-ciscoconfparse
endef

define Package/python3-ciscocfg/description
	Mareel Cisco Configuration parser
endef

$(eval $(call Py3Package,python3-ciscocfg))
$(eval $(call BuildPackage,python3-ciscocfg))
$(eval $(call BuildPackage,python3-ciscocfg-src))
