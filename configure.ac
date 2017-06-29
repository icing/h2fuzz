# Copyright greenbytes GmbH (https://www.greenbytes.de)
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

AC_PREREQ([2.67])
AC_INIT([h2fuzz], [0.0.1-DEV], [stefan.eissing@greenbytes.de])

LT_PREREQ([2.2.6])
LT_INIT()
dnl See versioning rule:
dnl  http://www.gnu.org/software/libtool/manual/html_node/Updating-version-info.html
AC_SUBST(LT_CURRENT, 11)
AC_SUBST(LT_REVISION, 0)
AC_SUBST(LT_AGE, 6)

major=`echo $PACKAGE_VERSION |cut -d. -f1 | sed -e "s/[^0-9]//g"`
minor=`echo $PACKAGE_VERSION |cut -d. -f2 | sed -e "s/[^0-9]//g"`
patch=`echo $PACKAGE_VERSION |cut -d. -f3 | cut -d- -f1 | sed -e "s/[^0-9]//g"`

PACKAGE_VERSION_NUM=`printf "0x%02x%02x%02x" "$major" "$minor" "$patch"`

AC_SUBST(PACKAGE_VERSION_NUM)

AC_CANONICAL_BUILD
AC_CANONICAL_HOST
AC_CANONICAL_TARGET

AC_CONFIG_MACRO_DIR([m4])

AM_INIT_AUTOMAKE([subdir-objects no-define])
AM_EXTRA_RECURSIVE_TARGETS([update])

m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

OS=$( uname -s )

export BUILD_SUBDIRS=""

AC_MSG_NOTICE("configuring build")

# Checks for programs.
AC_PROG_CC
AC_PROG_CC_STDC

INSTALL_DEP=""
INSTALL_ADDITIONAL=""
TEST_DEP=""

################################################################################
# honggfuzz 
################################################################################

HF_BRANCH=master

BUILD_SUBDIRS="$BUILD_SUBDIRS honggfuzz"

HF_HOME="$PWD/honggfuzz/gen/honggfuzz"
HF_CC="${HF_HOME}"/hfuzz_cc/hfuzz-clang
HF_CFLAGS="-fsanitize=address -ggdb -fno-builtin -fno-inline -funroll-loops"

HONGGFUZZ="${HF_HOME}/honggfuzz"

################################################################################
# nghttp2 
################################################################################

NGHTTP2_VERSION=1.23.1
NGHTTP2_DEPS=""

BUILD_SUBDIRS="$BUILD_SUBDIRS nghttp2"
INSTALL_DEP="install-nghttp2 ${INSTALL_DEP}"
INSTALL_ADDITIONAL="$INSTALL_ADDITIONAL nghttp2($NGHTTP2_VERSION)"

################################################################################
# httpd 
################################################################################

HTTPD_BRANCH=trunk
HTTPD_ROOT="${prefix}"

BUILD_SUBDIRS="$BUILD_SUBDIRS httpd"
INSTALL_DEP="install-httpd ${INSTALL_DEP}"
INSTALL_ADDITIONAL="$INSTALL_ADDITIONAL httpd($HTTPD_BRANCH)"

################################################################################
# generate files 
################################################################################

# substitution in generated files
AC_SUBST(BUILD_SUBDIRS)
AC_SUBST(SYSCONF_DIR)
AC_SUBST(LIB_DIR)
AC_SUBST(LIBEXEC_DIR)
AC_SUBST(SERVER_NAME)
AC_SUBST(ABS_PWD)
AC_SUBST(APACHECTL)
AC_SUBST(HTTPD)
AC_SUBST(MODULES)
AC_SUBST(APR_DEPS)
AC_SUBST(APR_VERSION)
AC_SUBST(APR_UTIL_DEPS)
AC_SUBST(APR_UTIL_VERSION)
AC_SUBST(OPENSSL_DEPS)
AC_SUBST(OPENSSL_VERSION)
AC_SUBST(OPENSSL_CONF_CMD)
AC_SUBST(NGHTTP2_DEPS)
AC_SUBST(NGHTTP2_VERSION)
AC_SUBST(HTTPD_BRANCH)
AC_SUBST(HTTPD_ROOT)
AC_SUBST(HF_BRANCH)
AC_SUBST(HF_CC)
AC_SUBST(HF_CFLAGS)
AC_SUBST(HF_HOME)
AC_SUBST(HONGGFUZZ)
AC_SUBST(INSTALL_DEP)
AC_SUBST(TEST_DEP)

AC_CONFIG_FILES([
    Makefile
    honggfuzz/Makefile
    nghttp2/Makefile
    httpd/Makefile
    conf/httpd-h2.conf
])

AC_OUTPUT

AC_MSG_NOTICE([summary of build options:

    Version:          ${VERSION} shared $LT_CURRENT:$LT_REVISION:$LT_AGE
    Host type:        ${host}
    prefix:           ${prefix}
    NGHTTP:           ${NGHTTP_INSTALLED}
    download+install: ${INSTALL_ADDITIONAL:-n.a.}
])
