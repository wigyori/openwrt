include $(TOPDIR)/rules.mk

PKG_SOURCE_URL:=https://github.com/espressif/esp-hosted
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2026-02-08
PKG_SOURCE_VERSION:=1f340824158c5e12478f92c9d7a06a75cdfdb065
PKG_MIRROR_HASH:=033418cb38a1dcc3487ccdb33cf7ad289ce1f0dee9db8a2acffc182da4130395

PKG_MAINTAINER:=Zoltan HERPAI <wigyori@uid0.hu>

PKG_NAME:=esp-hosted
PKG_VERSION:=1.0
PKG_RELEASE=1

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=
PKG_USE_NINJA:=0
PKG_BUILD_PARALLEL:=1

PKG_CONFIG_DEPENDS += \
	CONFIG_PACKAGE_CFG80211_TESTMODE

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk


define KernelPackage/esp-default
  SUBMENU:=Wireless Drivers
  DEPENDS:= \
	+kmod-mac80211 \
	+@KERNEL_PAGE_POOL \
	@!TARGET_uml
endef

define KernelPackage/esp32-sdio
	$(KernelPackage/esp-default)
	SUBMENU:=Wireless Drivers
	TITLE:=ESP32 SDIO wireless driver (metapackage)
	DEPENDS:= \
		+kmod-mmc +kmod-bluetooth +kmod-mac80211 +kmod-cfg80211
	FILES:=$(PKG_BUILD_DIR)/esp_hosted_ng/host/esp32_sdio.ko
	AUTOLOAD:=$(call AutoProbe,esp32_sdio)
	PROVIDES:=kmod-esp32-sdio
endef

#define KernelPackage/esp32-spi
#	$(KernelPackage/esp-default)
#	SUBMENU:=Wireless Drivers
#	TITLE:=ESP32 SPI wireless driver (metapackage)
#	DEPENDS:= \
#		+kmod-bluetooth +kmod-mac80211 +kmod-cfg80211
#	FILES:=$(PKG_BUILD_DIR)/esp_hosted_ng/host/esp32_spi.ko
#	AUTOLOAD:=$(call AutoProbe,esp32_spi)
#	PROVIDES:=kmod-esp32-sdio
#endef

#define KernelPackage/esp32-spi/description
#  This package contains the ESP32 SPI wireless driver for OpenWrt.
#  It enables WiFi connectivity using ESP32 modules connected via SPI interface.
#endef

TARGET_CFLAGS += -I$(STAGING_DIR)/usr/include/libnl-tiny

NOSTDINC_FLAGS := \
	$(KERNEL_NOSTDINC_FLAGS) \
	-I$(PKG_BUILD_DIR) \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-include backport/autoconf.h \
	-include backport/backport.h

ifdef CONFIG_PACKAGE_MAC80211_MESH
  NOSTDINC_FLAGS += -DCONFIG_MAC80211_MESH
endif

ifdef CONFIG_PACKAGE_CFG80211_TESTMODE
  NOSTDINC_FLAGS += -DCONFIG_NL80211_TESTMODE
  PKG_MAKE_FLAGS += CONFIG_NL80211_TESTMODE=y
endif

ifdef CONFIG_PACKAGE_kmod-esp32-sdio
  PKG_MAKE_FLAGS += CONFIG_ESP_SDIO=m
endif

# makefile in esp-hosted only supports to build for "one" target at a time - need some patching
#ifdef CONFIG_PACKAGE_kmod-esp32-spi
#  PKG_MAKE_FLAGS += CONFIG_ESP_SPI=m
#endif

define Build/Compile
	+$(KERNEL_MAKE) $(PKG_JOBS) \
		$(PKG_MAKE_FLAGS) \
		M="$(PKG_BUILD_DIR)/esp_hosted_ng/host" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		modules
endef

$(eval $(call KernelPackage,esp32-sdio))
#$(eval $(call KernelPackage,esp32-spi))
