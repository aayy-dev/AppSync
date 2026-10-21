# Directory of this Makefile (captured before Theos includes) so vendored
# paths resolve regardless of the working directory.
APPINST_DIR := $(dir $(realpath $(lastword $(MAKEFILE_LIST))))

ifeq ($(THEOS_PACKAGE_SCHEME),rootless)
export TARGET = iphone:clang:latest:15.0
export ARCHS = arm64 arm64e
else
export TARGET = iphone:clang:latest:5.0
export ARCHS = armv7 armv7s arm64
endif
export DEBUG = 0

THEOS_PACKAGE_DIR_NAME = debs
PACKAGE_VERSION = $(THEOS_PACKAGE_BASE_VERSION)

include $(THEOS)/makefiles/common.mk

TOOL_NAME = appinst
appinst_FILES = appinst.m
appinst_CCFLAGS += -std=c++11 -stdlib=libc++ -fobjc-arc -include ./zip.h -I . -fvisibility=hidden -Wno-unused-property-ivar
appinst_CFLAGS += -fobjc-arc -include ./zip.h -I . -fvisibility=hidden -Wno-unused-property-ivar
appinst_FRAMEWORKS = Foundation ImageIO CoreGraphics
appinst_PRIVATE_FRAMEWORKS = MobileCoreServices
# A static libzip (1.7.3, arm64+arm64e) vendored next to this Makefile so CI and
# fresh checkouts build without a separate libzip install. Override with LIBZIP=.
LIBZIP ?= $(APPINST_DIR)libzip.a
appinst_LDFLAGS += $(LIBZIP)
appinst_LIBRARIES = z
appinst_INSTALL_PATH = /usr/bin
appinst_CODESIGN_FLAGS = -Sappinst_entitlements.plist

include $(THEOS_MAKE_PATH)/tool.mk

package::
ifndef THEOS_PACKAGE_SCHEME
	@$(_THEOS_PLATFORM_DPKG_DEB) -b -Zgzip "transitional/nodelete-com.linusyang.appinst" "$(THEOS_PACKAGE_DIR_NAME)/nodelete-com.linusyang.appinst.deb"
endif
