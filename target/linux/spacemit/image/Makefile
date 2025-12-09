# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2025 Toco Technologies <info@toco.ae>
#
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/image.mk

FAT32_BLOCK_SIZE=1024
FAT32_BLOCKS=$(shell echo $$(($(CONFIG_SPACEMIT_SD_BOOT_PARTSIZE)*1024*1024/$(FAT32_BLOCK_SIZE))))

KERNEL_LOADADDR:=0x80200000

define Build/riscv-sdcard
	rm -f $@.boot #$(KDIR_TMP)/$(IMG_PREFIX)-$(PROFILE)-boot.img
	mkfs.fat $@.boot  -C $(FAT32_BLOCKS)

#	mcopy -i $@.boot $(STAGING_DIR_IMAGE)/$(DEVICE_NAME)-boot.scr ::boot.scr
	mcopy -i $@.boot $(DTS_DIR)/$(DEVICE_DTS).dtb ::dtb
	mcopy -i $@.boot $(IMAGE_KERNEL) ::Image
	mcopy -i $@.boot files/boot.scr.uimg ::boot.scr.uimg

	./gen_spacemit_sdcard_img.sh \
		$@ \
		$@.boot \
		$(IMAGE_ROOTFS) \
		$(CONFIG_SPACEMIT_SD_BOOT_PARTSIZE) \
		$(CONFIG_TARGET_ROOTFS_PARTSIZE)
#		$(STAGING_DIR_IMAGE)/$(DEVICE_NAME)-u-boot.itb \
#		$(STAGING_DIR_IMAGE)/$(DEVICE_NAME)-u-boot.itb-spl
endef

define Device/Default
  PROFILES := Default
  KERNEL_NAME := Image
  KERNEL := kernel-bin
  IMAGES := rootfs.img.gz
  IMAGE/rootfs.img.gz := append-rootfs | gzip
  IMAGES := sdcard.img.gz
  IMAGE/sdcard.img.gz := riscv-sdcard | append-metadata | gzip
endef

define Device/bananapi_f3
  DEVICE_VENDOR := BananaPi
  DEVICE_MODEL := F3
  DEVICE_DTS := spacemit/k1-bananapi-f3
  UBOOT := bananapi-f3
endef
TARGET_DEVICES += bananapi_f3

define Device/milkv_jupiter
  DEVICE_VENDOR := Milk-V
  DEVICE_MODEL := Jupiter
  DEVICE_DTS := spacemit/k1-milkv-jupiter
  UBOOT := milkv_jupiter
endef
TARGET_DEVICES += milkv_jupiter

define Device/orangepi_rv2
  DEVICE_VENDOR := OrangePi
  DEVICE_MODEL := RV2 (K1)
  DEVICE_DTS := spacemit/k1-orangepi-rv2
  UBOOT := orangepi_rv2
endef
TARGET_DEVICES += orangepi_rv2

define Device/orangepi_r2s
  DEVICE_VENDOR := OrangePi
  DEVICE_MODEL := R2S
  DEVICE_DTS := spacemit/k1-orangepi-r2s
  UBOOT := orangepi_r2s
endef
TARGET_DEVICES += orangepi_r2s

$(eval $(call BuildImage))
