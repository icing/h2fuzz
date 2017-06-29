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
DIST_SUBDIRS    = nghttp2 httpd

ACLOCAL_AMFLAGS = -I m4

EXTRA_DIST = \
    nghttp2/Makefile 


HTTP_PORT      = 12345

GEN            = gen

APACHECTL      = @APACHECTL@ -d $(SERVER_DIR)
HTTPD          = @HTTPD@
HONGGFUZZ      = @HONGGFUZZ@
HF_APACHE      = @HF_HOME@/examples/apache

.PHONY: clean


all: install

clean:
	@rm -rf $(GEN) apr/gen apr-util/gen nghttp2/gen httpd/gen

distdir:
	@mkdir -p $(distdir)
	@tar cf - Makefile bin *.txt *.sh | (cd $(distdir) && tar xf - )

fuzz: install
	$(HONGGFUZZ) -z -P -f $(HF_APACHE)/corpus_http2 -w $(HF_APACHE)/httpd.wordlist -- $(prefix)/bin/httpd -X -f $(prefix)/conf/httpd.conf.h2
