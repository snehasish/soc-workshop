
SHELL := /bin/bash

ifndef WORK_ROOT
	WORK_ROOT = ./build
endif

ifndef INSTALL_DIR
	INSTALL_DIR = ./install
endif

################################################################################
# Tools

CAT := cat
CD := cd
CHMOD := chmod
CP := cp -rf
ECHO := echo
DATE := date
HEAD := head
MKDIR := mkdir -p
MV := mv
RM := rm -rf
SED := sed
TAR := tar
TOUCH := touch
FIND := find
PATCH := patch
LN := ln

# Helpful Macros
SPACE := $(empty) $(empty)

################################################################################

LINUX_VARIABLES = PATH=$(PATH)
LINUX_VARIABLES += ARCH=$(ARCH)
ifneq ("$(KBUILD_BUILD_VERSION)","")
	LINUX_VARIABLES += KBUILD_BUILD_VERSION="$(KBUILD_BUILD_VERSION)" 
endif
LINUX_VARIABLES += CROSS_COMPILE=$(CROSS_COMPILE)
ifneq ("$(DEVICETREE_SRC)","")
	LINUX_VARIABLES += CONFIG_DTB_SOURCE=$(DEVICETREE_SRC)
endif
LINUX_VARIABLES += INSTALL_MOD_PATH=$(CURDIR)/../overlay

#	SW_SRC_INSTALL_DEPS := mmlink
#	SW_SRC_INSTALL_DEPS += msgdma_memtest
	SW_SRC_INSTALL_DEPS += validator_module
	SW_SRC_INSTALL_DEPS += validator_devmem
	SW_SRC_INSTALL_DEPS += acl_module
#	SW_SRC_INSTALL_DEPS += sld_hub_csr_module
	SW_SRC_INSTALL_DEPS += ws1_lab_validator
	SW_SRC_INSTALL_DEPS += ws2_lab_validator
	SW_SRC_INSTALL_DEPS += ws3_lab_validator
	SW_SRC_INSTALL_DEPS += demo_devmem_app
	SW_SRC_INSTALL_DEPS += demo_ioctl_test_app
	SW_SRC_INSTALL_DEPS += demo_map_test_app
	SW_SRC_INSTALL_DEPS += demo_uio_test_app
	SW_SRC_INSTALL_DEPS += driver_modules

.PHONY: all
all: install

.PHONY: install
install: $(SW_SRC_INSTALL_DEPS)
	$(MKDIR) $(INSTALL_DIR)/usr/bin
#	$(CP) mmlink/mmlink $(INSTALL_DIR)/usr/bin
#	$(CP) msgdma_user_space/memtest $(INSTALL_DIR)/examples
	$(CP) validator_devmem_app/validator_devmem $(INSTALL_DIR)/examples/validator
	$(MKDIR) $(INSTALL_DIR)/examples/fft
	$(CP) ws1_lab_validator/ws1_lab_validator $(INSTALL_DIR)/examples/fft
	$(CP) ws1_lab_validator/ws1_lab_validation_check.sh $(INSTALL_DIR)/examples/fft
	$(MKDIR) $(INSTALL_DIR)/examples/workshop_2
	$(CP) ws2_lab_validator/ws2_lab_validator $(INSTALL_DIR)/examples/workshop_2
	$(CP) ws2_lab_validator/ws2_lab_validation_check.sh $(INSTALL_DIR)/examples/workshop_2
	$(MKDIR) $(INSTALL_DIR)/examples/drivers
	$(CP) ws3_target_tests/README.TXT $(INSTALL_DIR)/examples/drivers
	$(CP) ws3_target_tests/target_test_devmem.sh $(INSTALL_DIR)/examples/drivers
	$(CP) ws3_lab_validator/ws3_lab_validator $(INSTALL_DIR)/examples/drivers
	$(CP) ws3_lab_validator/ws3_lab_validation_check.sh $(INSTALL_DIR)/examples/drivers
	$(CP) demo_devmem_app/demo_devmem $(INSTALL_DIR)/examples/drivers
	$(CP) demo_ioctl_test_app/ioctl_test $(INSTALL_DIR)/examples/drivers
	$(CP) demo_map_test_app/demo_map_test $(INSTALL_DIR)/examples/drivers
	$(CP) demo_uio_test_app/demo_uio_test $(INSTALL_DIR)/examples/drivers
#	make -C sld_hub_csr_module OUT_DIR=../../$(LINUX_BRANCH) $(LINUX_VARIABLES) modules_install
	make -C validator_module OUT_DIR=../../$(LINUX_BRANCH) $(LINUX_VARIABLES) modules_install
	make -C acl OUT_DIR=../../$(LINUX_BRANCH) $(LINUX_VARIABLES) modules_install
	make -C driver_modules OUT_DIR=../../$(LINUX_BRANCH) $(LINUX_VARIABLES) modules_install

.PHONY: mmlink
mmlink: 
	make -C mmlink KERNEL_SRC_DIR=$(CURDIR)/../$(LINUX_BRANCH) CROSS_COMPILE=$(CROSS_COMPILE)	

.PHONY: msgdma_memtest
msgdma_memtest:
	make -C msgdma_user_space CROSS_COMPILE=$(CROSS_COMPILE)

.PHONY: validator_devmem	
validator_devmem:
	make -C validator_devmem_app CROSS_COMPILE=$(CROSS_COMPILE)

.PHONY: validator_module
validator_module:
	make -C validator_module OUT_DIR=$(CURDIR)/../$(LINUX_BRANCH) $(LINUX_VARIABLES) modules

.PHONY: acl_module
acl_module:
	make -C acl OUT_DIR=$(CURDIR)/../$(LINUX_BRANCH) $(LINUX_VARIABLES) all


.PHONY: sld_hub_csr_module
sld_hub_csr_module:
	make -C sld_hub_csr_module OUT_DIR=$(CURDIR)/../$(LINUX_BRANCH) $(LINUX_VARIABLES) modules

.PHONY: driver_modules
driver_modules:
	make -C driver_modules OUT_DIR=$(CURDIR)/../$(LINUX_BRANCH) $(LINUX_VARIABLES) modules
	
.PHONY: ws1_lab_validator
ws1_lab_validator:
	make -C ws1_lab_validator CROSS_COMPILE=$(CROSS_COMPILE)

.PHONY: ws2_lab_validator
ws2_lab_validator:
	make -C ws2_lab_validator CROSS_COMPILE=$(CROSS_COMPILE)

.PHONY: ws3_lab_validator
ws3_lab_validator:
	make -C ws3_lab_validator CROSS_COMPILE=$(CROSS_COMPILE)

.PHONY: demo_devmem_app	
demo_devmem_app:
	make -C demo_devmem_app CROSS_COMPILE=$(CROSS_COMPILE)

.PHONY: demo_ioctl_test_app	
demo_ioctl_test_app:
	make -C demo_ioctl_test_app CROSS_COMPILE=$(CROSS_COMPILE)

.PHONY: demo_map_test_app	
demo_map_test_app:
	make -C demo_map_test_app CROSS_COMPILE=$(CROSS_COMPILE)

.PHONY: demo_uio_test_app	
demo_uio_test_app:
	make -C demo_uio_test_app CROSS_COMPILE=$(CROSS_COMPILE)

.PHONY: clean
clean:
	make -C mmlink clean
	make -C msgdma_user_space clean
	make -C validator_devmem_app clean
	make -C validator_module OUT_DIR=$(CURDIR)/../$(LINUX_BRANCH) clean
	make -C sld_hub_csr_module OUT_DIR=$(CURDIR)/../$(LINUX_BRANCH) clean
	make -C driver_modules OUT_DIR=$(CURDIR)/../$(LINUX_BRANCH) clean
	make -C ws1_lab_validator clean
	make -C ws2_lab_validator clean
	make -C ws3_lab_validator clean
	make -C demo_devmem_app clean
	make -C demo_ioctl_test_app clean
	make -C demo_map_test_app clean
	make -C demo_uio_test_app clean
