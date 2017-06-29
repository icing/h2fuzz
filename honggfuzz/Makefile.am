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

HF_BRANCH       = @HF_BRANCH@
HF_DIR          = honggfuzz
HF_ZIP          = $(HF_BRANCH).zip
HF_URL          = https://github.com/google/honggfuzz/archive/$(HF_ZIP)
HF_GH_SSH       = git@github.com:google/honggfuzz.git

HF_CONF         = 
HF_CONF_ENV     =  


all:	install 

dirs:
	@mkdir -p $(GEN)/build

clean:
	@rm -rf $(GEN)/.installed $(GEN)/.built
	-@cd $(GEN)/$(HF_DIR) && make clean

distclean:
	@rm -rf $(GEN)

install:  $(GEN)/.built

install-recursive: 

################################################################################
# Install the local honggfuzz
#
$(GEN)/.installed:  $(GEN)/.built
	@echo -n installing honggfuzz locally...
	@cd $(GEN)/$(HF_DIR)/ && make install
	@echo done.
	@touch $(GEN)/.installed

################################################################################
# Build the local honggfuzz
#
$(GEN)/.built: \
		$(GEN)/.configured
	@echo -n building honggfuzz...
	@cd $(GEN)/$(HF_DIR)/ && make
	@echo done.
	@touch $(GEN)/.built

################################################################################
# Configure the local honggfuzz sources
#
$(GEN)/.configured: \
		$(GEN)/.cloned
	@echo -n configuring honggfuzz...
	cd $(GEN)/$(HF_DIR)/ && \
	$(HF_CONF_ENV) echo "nothing to configure so far"
	@echo done.
	@touch $(GEN)/.configured

################################################################################
# Clone from github
#
$(GEN)/.cloned: 
	@rm -rf $(GEN)/$(HF_DIR)
	@mkdir -p $(GEN)
	@echo -n cloning from github
	@cd $(GEN) && git clone $(HF_GH_SSH)
	@cd $(GEN)/honggfuzz && git checkout $(HF_BRANCH)
	@echo done.
	@touch $(GEN)/.cloned

################################################################################
# Extract honggfuzz source tree
#
$(GEN)/.extracted: \
		$(GEN)/$(HF_TAR)
	@rm -rf $(GEN)/$(HF_DIR)
	@echo -n extracting honggfuzz packages...
	@cd $(GEN) && unzip $(HF_ZIP)
	@echo done.
	@touch $(GEN)/.extracted

################################################################################
# Retrieve honggfuzz sources
#
$(GEN)/$(HF_TAR):
	@mkdir -p $(GEN)
	cd $(GEN) && curl $(CURL_OPTS) -L -O $(HF_URL)

