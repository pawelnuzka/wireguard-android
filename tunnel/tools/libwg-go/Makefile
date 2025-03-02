# SPDX-License-Identifier: Apache-2.0
#
# Copyright © 2017-2019 WireGuard LLC. All Rights Reserved.

# Output directories
BUILDDIR ?= $(CURDIR)/build
DESTDIR ?= $(CURDIR)/out

# Architecture mappings for Android
NDK_GO_ARCH_MAP_x86 := 386
NDK_GO_ARCH_MAP_x86_64 := amd64
NDK_GO_ARCH_MAP_arm := arm
NDK_GO_ARCH_MAP_arm64 := arm64
NDK_GO_ARCH_MAP_mips := mipsx
NDK_GO_ARCH_MAP_mips64 := mips64x

# Clang flags passed to CGO
CLANG_FLAGS := --target=$(ANDROID_LLVM_TRIPLE) --gcc-toolchain=$(ANDROID_TOOLCHAIN_ROOT) --sysroot=$(ANDROID_SYSROOT)
export CGO_CFLAGS := $(CLANG_FLAGS) $(CFLAGS)
export CGO_LDFLAGS := $(CLANG_FLAGS) $(LDFLAGS) -Wl,-soname=libwg-go.so
export CC := $(ANDROID_C_COMPILER)

# Set GOOS, GOARCH and enable CGO
export GOARCH := $(NDK_GO_ARCH_MAP_$(ANDROID_ARCH_NAME))
export GOOS := android
export CGO_ENABLED := 1

# Instead of downloading a Go tarball, use the installed Go binary.
# Make sure Go 1.24 is installed at /opt/homebrew/bin/go.
GO_BIN := /opt/homebrew/bin/go

# The default target builds libwg-go.so
default: $(DESTDIR)/libwg-go.so

$(DESTDIR)/libwg-go.so: go.mod
	@mkdir -p "$(DESTDIR)"
	$(GO_BIN) build -tags linux \
		-ldflags="-X golang.zx2c4.com/wireguard/ipc.socketDirectory=/data/data/$(ANDROID_PACKAGE_NAME)/cache/wireguard" \
		-v -trimpath -o "$@" -buildmode c-shared

.DELETE_ON_ERROR: