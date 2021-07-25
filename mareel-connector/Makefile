# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_GROUP:=@mareel
PKG_NPM_NAME:=core
PKG_NAME:=mareel-connector
PKG_VERSION:=0.4.2
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NPM_NAME)-$(PKG_VERSION).tgz
PKG_SOURCE_URL:=https://registry.npmjs.org/$(PKG_NPM_GROUP)/$(PKG_NPM_NAME)/-/
PKG_HASH:=764ea032807d1841dc452796c8a67b9f519cd58f3c1bf5bce0cebd8c05f4191f

PKG_MAINTAINER:=Empo Inc. <admin@empo.im>
PKG_LICENSE:=GPL-3
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=node/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk

define Package/mareel-connector
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Mareel Connector
  URL:=https://mareel.io/
  DEPENDS:=+node +python3 +libpthread +python3-ciscocfg
endef

define Package/mareel-connector/description
 Mareel Connector Daemon
endef

TAR_OPTIONS+= --strip-components 1
TAR_CMD=$(HOST_TAR) -C $(1) $(TAR_OPTIONS)

NODEJS_CPU:=$(subst powerpc,ppc,$(subst aarch64,arm64,$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))))
TMPNPM:=$(shell mktemp -u XXXXXXXXXX)

TARGET_CFLAGS+=$(FPIC)
TARGET_CPPFLAGS+=$(FPIC)

define Build/Compile
	cd $(PKG_BUILD_DIR); \
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_nodedir=$(STAGING_DIR)/usr/ \
	npm_config_cache=$(TMP_DIR)/npm-cache-$(TMPNPM) \
	npm_config_tmp=$(TMP_DIR)/npm-tmp-$(TMPNPM) \
	npm install --production --global-style --no-save --omit=dev --no-package-lock --build-from-source --target_arch=$(NODEJS_CPU)
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
	$(CP) $(PKG_BUILD_DIR)/{cli,dist,mibjson,node_modules} \
		$(1)/usr/lib/node/$(PKG_NPM_GROUP)/$(PKG_NPM_NAME)/
	$(INSTALL_DIR) $(1)/usr/bin
	$(LN) ../lib/node/$(PKG_NPM_GROUP)/$(PKG_NPM_NAME)/cli/connectord.js $(1)/usr/bin/connectord
	$(INSTALL_DIR) $(1)/usr/lib/node_modules/$(PKG_NPM_GROUP)
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

$(eval $(call BuildPackage,mareel-connector))
