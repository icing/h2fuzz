# Copyright 2017 greenbytes GmbH (https://www.greenbytes.de)
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

GEN             = gen
CURL_OPTS       = -k --progress-bar

HTTPD_BRANCH    = @HTTPD_BRANCH@
HTTPD_ROOT      = @HTTPD_ROOT@
HTTPD_DIR       = httpd-$(HTTPD_BRANCH)
HTTPD_SVN_DIR   = @HTTPD_SVN_DIR@
HTTPD_URL       = https://svn.apache.org/repos/asf/httpd/httpd/$(HTTPD_SVN_DIR)

HTTPD_CONF      = --prefix=$(prefix) \
	--enable-maintainer-mode \
	--enable-nghttp2-staticlib-deps \
	--with-nghttp2=$(prefix) \
	--with-mpm=event --enable-unixd --disable-pie \
	--enable-mods-static=most \
	--enable-http2 --enable-cache --enable-cache-socache --enable-socache-shmcb --disable-ssl 

#HTTPD_CONF_ENV  = CC="@HF_CC@" CXX="@HF_CC@" CFLAGS="-ggdb O3 -fsanitize=address" LDFLAGS="-lpthread -lasan"
HTTPD_CONF_ENV  = CC="@HF_CC@" CXX="@HF_CC@" CFLAGS="@HF_CFLAGS@" LDFLAGS="@HF_CFLAGS@"

APR_VERSION     = @APR_VERSION@
APR_UTIL_VERSION= @APR_UTIL_VERSION@

all: install

dirs:
	@mkdir -p $(GEN)/build

clean:
	@rm -rf $(GEN)/.built $(GEN)/.configured $(HTTPD_ROOT)/.installed
	-@cd $(GEN)/$(HTTPD_DIR) && make clean

distclean:
	@rm -rf $(GEN) $(HTTPD_ROOT)

distdir:
	@mkdir -p $(distdir)
	@tar cf - Makefile patches | (cd $(distdir) && tar xf - )

install:  $(HTTPD_ROOT)/.installed

update:
	@echo "svn update $(HTTPD_DIR)..."
	@svn update $(GEN)/$(HTTPD_DIR)/
	@svn update $(GEN)/$(HTTPD_DIR)/srclib/apr
	@svn update $(GEN)/$(HTTPD_DIR)//srclib/apr-util
	
################################################################################
# Install the local httpd
#
$(HTTPD_ROOT)/.installed:  $(GEN)/.built
	@echo -n installing httpd locally...
	@cd $(GEN)/$(HTTPD_DIR)/ && make install
	@echo done.
	@touch $(HTTPD_ROOT)/.installed

################################################################################
# Build the local httpd
#
$(GEN)/.built: \
		$(GEN)/.configured
	@echo -n building httpd...
	@cd $(GEN)/$(HTTPD_DIR)/ && make
	@echo done.
	@touch $(GEN)/.built

################################################################################
# Configure the local httpd sources
#
$(GEN)/.configured: \
		$(GEN)/.patched
	@echo -n configuring httpd...
	cd $(GEN)/$(HTTPD_DIR)/ && $(HTTPD_CONF_ENV) ./configure $(HTTPD_CONF)
	@echo done.
	@touch $(GEN)/.configured

################################################################################
# Patch the local httpd sources
#
$(GEN)/.patched: \
		$(GEN)/.checkout
	@echo -n patching httpd...
	rm -f $(GEN)/$(HTTPD_DIR)/compile.sh
	svn revert -R $(GEN)/$(HTTPD_DIR)/
	test -f httpd-$(HTTPD_BRANCH).patch && patch -p0 -d $(GEN)/$(HTTPD_DIR)/  < httpd-$(HTTPD_BRANCH).patch
	@echo done.
	@touch $(GEN)/.patched

################################################################################
# Checkout apr source tree
#
$(GEN)/.checkout: 
	@rm -rf $(GEN)/$(HTTPD_DIR)
	@echo -n checkout httpd sources...
	svn co $(HTTPD_URL) $(GEN)/$(HTTPD_DIR)
	svn co http://svn.apache.org/repos/asf/apr/apr/branches/1.5.x $(GEN)/$(HTTPD_DIR)/srclib/apr
	svn co http://svn.apache.org/repos/asf/apr/apr-util/branches/1.5.x $(GEN)/$(HTTPD_DIR)/srclib/apr-util
	cd $(GEN)/$(HTTPD_DIR) && ./buildconf
	@echo done.
	@touch $(GEN)/.checkout

