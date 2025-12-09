# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2025 Toco Technologies <info@toco.ae>
#
include $(TOPDIR)/rules.mk

ARCH:=riscv64
BOARD:=spacemit
BOARDNAME:=Spacemit-based RISC-V boards
FEATURES:=ext4 squashfs
KERNELNAME:=Image dtbs
SUBTARGETS:=generic

KERNEL_PATCHVER:=6.18

DEFAULT_PACKAGES += uboot-envtools
DEFAULT_PACKAGES += partx-utils mkf2fs f2fsck e2fsprogs

include $(INCLUDE_DIR)/target.mk

define Target/Description
	Build firmware images for the Spacemit K1-boards
endef

$(eval $(call BuildTarget))
