#
# CDDL HEADER START
#
# The contents of this file are subject to the terms of the
# Common Development and Distribution License (the "License").
# You may not use this file except in compliance with the License.
#
# You can obtain a copy of the license at usr/src/OPENSOLARIS.LICENSE
# or http://www.opensolaris.org/os/licensing.
# See the License for the specific language governing permissions
# and limitations under the License.
#
# When distributing Covered Code, include this CDDL HEADER in each
# file and include the License file at usr/src/OPENSOLARIS.LICENSE.
# If applicable, add the following below this CDDL HEADER, with the
# fields enclosed by brackets "[]" replaced with your own identifying
# information: Portions Copyright [yyyy] [name of copyright owner]
#
# CDDL HEADER END
#
# Copyright (c) 2011, 2014, Oracle and/or its affiliates. All rights reserved.
# Copyright (c) 2018, Michal Nowak
#
BUILD_BITS= 64_and_32
#USE_OPENSSL11= yes
include ../../../make-rules/shared-macros.mk

COMPONENT_NAME =        curl
COMPONENT_VERSION=      7.76.1
COMPONENT_SRC=          $(COMPONENT_NAME)-$(COMPONENT_VERSION)
COMPONENT_PROJECT_URL=  https://curl.haxx.se
COMPONENT_ARCHIVE=      $(COMPONENT_SRC).tar.xz
COMPONENT_ARCHIVE_HASH=	sha256:64bb5288c39f0840c07d077e30d9052e1cbb9fa6c2dc52523824cc859e679145
COMPONENT_ARCHIVE_URL=  $(COMPONENT_PROJECT_URL)/download/$(COMPONENT_ARCHIVE)
COMPONENT_SIG_URL=  	$(COMPONENT_PROJECT_URL)/download/$(COMPONENT_ARCHIVE).asc

include $(WS_TOP)/make-rules/common.mk

# Community code uses different defs
$(BUILD_DIR_32)/.configured:   CPPFLAGS += -D__ILP32=32
$(BUILD_DIR_64)/.configured:   CPPFLAGS += -D__LP64=64

GSSAPI_LIBDIR_32 = /usr/lib
GSSAPI_LIBDIR_64 = /usr/lib/$(MACH64)

CFLAGS += $(CPP_LARGEFILES)
#CFLAGS += `pkg-config --cflags libidn`
#CPPFLAGS += `pkg-config --cflags libidn`

CONFIGURE_ENV += CPPFLAGS="$(CPPFLAGS) -I/usr/include/openldap"

CONFIGURE_OPTIONS += --localstatedir=/var --enable-shared --disable-static
CONFIGURE_OPTIONS += --enable-http --enable-ftp
CONFIGURE_OPTIONS += --enable-file --enable-dict
CONFIGURE_OPTIONS += --enable-manual --disable-libgcc
CONFIGURE_OPTIONS += --enable-rtsp --enable-proxy
CONFIGURE_OPTIONS += --enable-telnet --enable-tftp --enable-pop3
CONFIGURE_OPTIONS += --enable-imap --enable-smtp
CONFIGURE_OPTIONS += --enable-ipv6 --enable-nonblocking
CONFIGURE_OPTIONS += --enable-thread --enable-verbose
CONFIGURE_OPTIONS += --disable-sspi --enable-crypto-auth
CONFIGURE_OPTIONS += --enable-cookies --disable-hidden-symbols
CONFIGURE_OPTIONS += --disable-soname-bump
CONFIGURE_OPTIONS += --enable-ldap --with-random=/dev/urandom
CONFIGURE_OPTIONS += --with-ssl=/usr --with-ldap-lib=ldap_r-2.4
CONFIGURE_OPTIONS += --with-lber-lib=lber-2.4
CONFIGURE_OPTIONS += --with-gssapi-includes=/usr/include/gssapi
CONFIGURE_OPTIONS += --with-gssapi-libs=$(GSSAPI_LIBDIR_$(BITS))
CONFIGURE_OPTIONS += --with-gssapi=/usr --without-ca-bundle
CONFIGURE_OPTIONS += --with-ca-path=/etc/openssl/certs
CONFIGURE_OPTIONS += --with-zlib=/usr --with-libidn=/usr
CONFIGURE_OPTIONS += --with-pic
CONFIGURE_OPTIONS += --with-libssh2
CONFIGURE_OPTIONS += --without-librtmp
CONFIGURE_OPTIONS += "curl_disallow_getifaddrs=yes"

LINT_FLAGS += -I$(SOURCE_DIR)/include

# Generate configuration scripts form .am-files
COMPONENT_PREP_ACTION = (cd $(@D) ; autoreconf -fi)

ENV = /usr/bin/env -i
COMPONENT_TEST_ENV += CC="$(CC)"
COMPONENT_TEST_ENV += USER=`id -un`
COMPONENT_TEST_ENV += PATH="$(GCC_ROOT)/bin:$(GNUBIN):$(PATH)"
COMPONENT_TEST_ENV += CFLAGS="$(CFLAGS)"
COMPONENT_TEST_ENV += LC_ALL=C
COMPONENT_TEST_ARGS+= -k -i

# Only the #'s of passed/failed tests are verified for change
COMPONENT_TEST_TRANSFORMS += \
	'-n ' \
	'-e "s/ during [0-9][0-9]* seconds.//" ' \
	'-e "/TESTDONE:/p" ' \
	'-e "/TESTFAIL:/p" ' \
	'-e "/ TOTAL:/p" ' \
	'-e "/ PASS:/p" ' \
	'-e "/ SKIP:/p" ' \
	'-e "/ XFAIL:/p" ' \
	'-e "/ FAIL:/p" ' \
	'-e "/ XPASS:/p" ' \
	'-e "/ ERROR:/p" '

# Auto-generated dependencies
REQUIRED_PACKAGES += compress/zstd
REQUIRED_PACKAGES += library/brotli
REQUIRED_PACKAGES += library/libgsasl
REQUIRED_PACKAGES += library/libidn2
REQUIRED_PACKAGES += library/libssh2
REQUIRED_PACKAGES += library/nghttp2
REQUIRED_PACKAGES += library/openldap
REQUIRED_PACKAGES += library/security/openssl
REQUIRED_PACKAGES += library/zlib
REQUIRED_PACKAGES += shell/ksh93
REQUIRED_PACKAGES += system/library
REQUIRED_PACKAGES += system/library/security/gss
