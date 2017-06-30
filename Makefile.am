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
SUBDIRS         = @BUILD_SUBDIRS@
DIST_SUBDIRS    = honggfuzz nghttp2 httpd

ACLOCAL_AMFLAGS = -I m4

EXTRA_DIST = \
    nghttp2/Makefile 


HTTP_PORT      = 12345

GEN            = gen

APACHECTL      = @APACHECTL@ -d $(SERVER_DIR)
HTTPD          = @HTTPD@
HTTPD_ROOT     = @HTTPD_ROOT@
HONGGFUZZ      = @HONGGFUZZ@
HF_APACHE      = @HF_HOME@/examples/apache

.PHONY: clean update


all: install

distdir:
	@mkdir -p $(distdir)
	@tar cf - Makefile bin *.txt *.sh | (cd $(distdir) && tar xf - )

update: update-recursive


fuzz: install $(HTTPD_ROOT)/conf/httpd-h2.conf
	@rm -f $(HTTPD_ROOT)/logs/*log
	$(HONGGFUZZ) -z -P -f $(HF_APACHE)/corpus_http2 -w $(HF_APACHE)/httpd.wordlist -- \
		$(HTTPD_ROOT)/bin/httpd -X -f conf/httpd-h2.conf

$(HTTPD_ROOT)/conf/httpd-h2.conf: conf/httpd-h2.conf
	@mkdir -p $(HTTPD_ROOT)/conf
	@cp conf/httpd-h2.conf $(HTTPD_ROOT)/conf/

