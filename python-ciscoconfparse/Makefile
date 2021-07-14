# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=python-ciscoconfparse
PKG_VERSION:=1.5.36
PKG_RELEASE:=1


PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://github.com/mpenning/ciscoconfparse.git
PKG_SOURCE_VERSION:=46fbf06734c9629837475f6a132cd8c74dec1e14
PKG_SOPURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_LICENSE:=GPL-3.0
PKG_LICENSE_FILES:=LICENSE

include ../pypi.mk
include $(INCLUDE_DIR)/package.mk
include ../python3-package.mk

define Package/python3-ciscoconfparse
	SUBMENU:=Python
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=ciscoconfparse
	URL:=https://github.com/mpenning/ciscoconfparse
	DEPENDS:=+python3-colorama +python3-passlib +python3-dns
endef

define Package/python3-ciscoconfparse/description
 ciscoconfparse is a Python library that helps you quickly parse and manipulate
 cisco and cisco-like config file
endef

$(eval $(call Py3Package,python3-ciscoconfparse))
$(eval $(call BuildPackage,python3-ciscoconfparse))
#$(eval $(call BuildPackage,python3-ciscoconfparse-src))
