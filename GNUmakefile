MESON_BUILD_DIR = build
topdir := $(shell realpath $(dir $(lastword $(MAKEFILE_LIST))))

# Project information (may be an easier way to get this from meson)
PROJECT_NAME = $(shell grep ^project $(topdir)/meson.build | cut -d "'" -f 2)
PROJECT_VERSION = $(shell grep version $(topdir)/meson.build | grep -E ',$$' | cut -d "'" -f 2)

# ninja may be called something else
NINJA := $(shell $(topdir)/utils/find-ninja.sh)
ifeq ($(NINJA),)
NINJA = $(error "*** unable to find a suitable `ninja' command")
endif

all: setup
	$(NINJA) -C $(MESON_BUILD_DIR) -v

setup:
	meson setup $(MESON_BUILD_DIR)

check: setup
	meson test -C $(MESON_BUILD_DIR) -v

clean:
	-rm -rf $(MESON_BUILD_DIR)

authors:
	git log --pretty="%an <%ae>" | sort -u | sed -e 's|^|- |g' | sed G > AUTHORS.md
	head -n $$(($$(wc -l < AUTHORS.md) - 1)) AUTHORS.md > AUTHORS.md.new
	mv AUTHORS.md.new AUTHORS.md

import:
	$(topdir)/utils/import-src.sh $(topdir)/upstream.conf

# Quiet errors about target arguments not being targets
%:
	@true
