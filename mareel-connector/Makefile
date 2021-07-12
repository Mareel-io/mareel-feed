# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_GROUP:=@mareel
PKG_NPM_NAME:=core
PKG_NAME:=mareel-connector
PKG_VERSION:=0.4.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NPM_NAME)-$(PKG_VERSION).tgz
PKG_SOURCE_URL:=https://registry.npmjs.org/$(PKG_NPM_GROUP)/$(PKG_NPM_NAME)/-/
PKG_HASH:=3bc21fd88c99f722369f3b51c57a8c53789407cc436d5591ee4b34999f3aa4b9

PKG_MAINTAINER:=Empo Inc. <admin@empo.im>
PKG_LICENSE:=GPL-3
PKG_LICENSE_FILES:=LICENSE

HOST_BUILD_DEPENDS:=node/host
HOST_BUILD_PARALLEL:=1

PKG_BUILD_DEPENDS:=node/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

define Package/mareel-connector
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Mareel Connector
  URL:=https://mareel.io/
  DEPENDS:=+node python3
endef

define Package/mareel-connector/description
 Mareel Connector Daemon
endef

TAR_OPTIONS+= --strip-components 1
TAR_CMD=$(HOST_TAR) -C $(1) $(TAR_OPTIONS)

NODEJS_CPU:=$(subst powerpc,ppc,$(subst aarch64,arm64,$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))))
TMPNPM:=$(shell mktemp -u XXXXXXXXXX)
HOSTTMPNPM:=$(shell mktemp -u XXXXXXXXXX)

TARGET_CFLAGS+=$(FPIC)
TARGET_CPPFLAGS+=$(FPIC)

define Build/Compile
	cd $(PKG_BUILD_DIR); \
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_nodedir=$(STAGING_DIR)/usr/ \
	npm_config_cache=$(TMP_DIR)/npm-cache-$(TMPNPM) \
	npm_config_tmp=$(TMP_DIR)/npm-tmp-$(TMPNPM) \
	npm install --production --global-style --no-save --omit=dev --no-package-lock
	rm -rf $(TMP_DIR)/npm-tmp-$(TMPNPM)
	rm -rf $(TMP_DIR)/npm-cache-$(TMPNPM)
	rm -f $(PKG_BUILD_DIR)/node_modules/.package-lock.json
	find $(PKG_BUILD_DIR)/node_modules -type d -empty -print0 | xargs -0 rmdir || true
endef

define Package/mareel-connector/install
	$(INSTALL_DIR) $(1)/usr/lib/node/$(PKG_NPM_GROUP)/$(PKG_NPM_NAME)
	$(CP) $(PKG_BUILD_DIR)/{package.json,LICENSE} \
		$(1)/usr/lib/node/$(PKG_NPM_GROUP)/$(PKG_NPM_NAME)/
	$(CP) $(PKG_BUILD_DIR)/README.md \
		$(1)/usr/lib/node/$(PKG_NPM_GROUP)/$(PKG_NPM_NAME)/
	$(CP) $(PKG_BUILD_DIR)/{bin,lib} \
		$(1)/usr/lib/node/$(PKG_NPM_GROUP)/$(PKG_NPM_NAME)/
	$(INSTALL_DIR) $(1)/usr/bin
	$(LN) ../lib/node/$(PKG_NPM_GROUP)/$(PKG_NPM_NAME)/cli/connectord.js $(1)/usr/bin/connectord
	$(INSTALL_DIR) $(1)/usr/lib/node_modules
	$(LN) ../node/$(PKG_NPM_GROUP)/$(PKG_NPM_NAME) $(1)/usr/lib/node_modules/$(PKG_NPM_GROUP)/$(PKG_NPM_NAME)
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DIR) $(1)/etc/mareel
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/etc/init.d/mareel-connector $(1)/etc/init.d/
	$(INSTALL_BIN) ./files/etc/mareel/connectord.yaml $(1)/etc/mareel/
endef

define Package/mareel-connector/postrm
#!/bin/sh
rm /usr/lib/node_modules/@mareel/core || true
rm -rf /usr/lib/node/@mareel/core || true
endef

define Host/Compile
	cd $(HOST_BUILD_DIR); \
	$(HOST_MAKE_VARS) \
	npm_config_nodedir=$(STAGING_DIR)/usr/ \
	npm_config_cache=$(TMP_DIR)/npm-cache-$(HOSTTMPNPM) \
	npm_config_tmp=$(TMP_DIR)/npm-tmp-$(HOSTTMPNPM) \
	npm install --production --global-style --no-save --omit=dev --no-package-lock
	rm -rf $(TMP_DIR)/npm-tmp-$(HOSTTMPNPM)
	rm -rf $(TMP_DIR)/npm-cache-$(HOSTTMPNPM)
	rm -f $(HOST_BUILD_DIR)/node_modules/.package-lock.json
	find $(HOST_BUILD_DIR)/node_modules -type d -empty -print0 | xargs -0 rmdir || true
endef

define Host/Install
	$(INSTALL_DIR) $(1)/lib/node_modules/$(PKG_NPM_GROUP)/$(PKG_NPM_NAME)
	$(CP) $(HOST_BUILD_DIR)/{package.json,LICENSE} \
		$(1)/lib/node_modules/$(PKG_NPM_GROUP)/$(PKG_NPM_NAME)/
	$(CP) $(HOST_BUILD_DIR)/README.md \
		$(1)/lib/node_modules/$(PKG_NPM_GROUP)/$(PKG_NPM_NAME)/
	$(CP) $(HOST_BUILD_DIR)/{bin,lib} \
		$(1)/lib/node_modules/$(PKG_NPM_GROUP)/$(PKG_NPM_NAME)/
	$(INSTALL_DIR) $(1)/bin
	$(LN) ../lib/node_modules/$(PKG_NPM_GROUP)/$(PKG_NPM_NAME)/cli/connectord.js $(1)/bin/connectord
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,mareel-connector))
