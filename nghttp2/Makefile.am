# Copyright 2015 greenbytes GmbH (https://www.greenbytes.de)
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
PYTHONPATH      = $(prefix)/lib/python2.7/site-packages

NGHTTP2_VERSION = @NGHTTP2_VERSION@
NGHTTP2_DIR     = nghttp2-$(NGHTTP2_VERSION)
NGHTTP2_TAR     = $(NGHTTP2_DIR).tar.gz
NGHTTP2_URL     = https://github.com/tatsuhiro-t/nghttp2/releases/download/v$(NGHTTP2_VERSION)/$(NGHTTP2_TAR)

NGHTTP2_DEPS    = @NGHTTP2_DEPS@
NGHTTP2_CONF    = --prefix=$(prefix) --enable-lib-only --enable-static --disable-shared
#NGHTTP2_CONF_ENV= CC="@HF_CC@" CXX="@HF_CC@" CFLAGS="@HF_CFLAGS@" LDFLAGS="@HF_CFLAGS@"

all: install

dirs:
	@mkdir -p $(GEN)/build

clean:
	@rm -rf $(GEN)/.installed $(GEN)/.built
	-@cd $(GEN)/$(NGHTTP2_DIR) && make clean

distclean:
	@rm -rf $(GEN)

distdir:
	@mkdir -p $(distdir)
	@tar cf - Makefile patches | (cd $(distdir) && tar xf - )

install:  $(GEN)/.installed


################################################################################
# Install the local nghttp2
#
$(GEN)/.installed:  $(GEN)/.built
	@echo -n installing nghttp2 locally...
	@mkdir -p $(PYTHONPATH)
	@cd $(GEN)/$(NGHTTP2_DIR)/ && PYTHONPATH=$(PYTHONPATH) make install
	@echo done.
	@touch $(GEN)/.installed


################################################################################
# Build the local nghttp2
#
$(GEN)/.built: \
		$(GEN)/.configured
	@echo -n building nghttp2...
	@cd $(GEN)/$(NGHTTP2_DIR)/ && make
	@echo done.
	@touch $(GEN)/.built

################################################################################
# Configure the local nghttp2 sources
#
$(GEN)/.configured: \
		$(NGHTTP2_DEPS) \
		$(GEN)/.patched
	@echo -n configuring nghttp2...
	cd $(GEN)/$(NGHTTP2_DIR)/ && \
	$(NGHTTP2_CONF_ENV) ./configure $(NGHTTP2_CONF)
	@echo done.
	@touch $(GEN)/.configured

################################################################################
# Patch the local nghtp2 sources
#
$(GEN)/.patched: \
		$(GEN)/.extracted
	@touch $(GEN)/.patched

################################################################################
# Extract nghttp2 source tree
#
$(GEN)/.extracted: \
		$(GEN)/$(NGHTTP2_TAR)
	@rm -rf $(GEN)/$(NGHTTP2_DIR)
	@echo -n extracting nghttp2 packages...
	@cd $(GEN) && tar xfz $(NGHTTP2_TAR)
	@echo done.
	@touch $(GEN)/.extracted

################################################################################
# Retrieve nghttp2 sources
#
# -k as the s3.amazonws certs have an unknown root (2015-10-07)
#
$(GEN)/$(NGHTTP2_TAR):
	@mkdir -p $(GEN)
	curl $(CURL_OPTS) -k -L $(NGHTTP2_URL) > $(GEN)/$(NGHTTP2_TAR) || rm -f $(GEN)/$(NGHTTP2_TAR)


