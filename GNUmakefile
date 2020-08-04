ROOT_DIR := $(shell dirname $(realpath $(firstword $(MAKEFILE_LIST))))
ALL_TARGETS := all base check install preinstall package rpm clean
MAKE_FILE := Makefile

DEFAULT_BUILD_DIR := build
BUILD_DIR := $(shell if [ -f $(MAKE_FILE) ]; then echo "."; else echo $(DEFAULT_BUILD_DIR); fi)
CMAKE3 := $(shell if which cmake3>/dev/null ; then echo cmake3; else echo cmake; fi;)

.PHONY: $(ALL_TARGETS)

all: base
	make -C $(BUILD_DIR) -f Makefile

base:
	make -C workflow
	mkdir -p $(BUILD_DIR)
ifeq ($(DEBUG),y)
	cd $(BUILD_DIR) && $(CMAKE3) -D CMAKE_BUILD_TYPE=Debug $(ROOT_DIR)
else
	cd $(BUILD_DIR) && $(CMAKE3) $(ROOT_DIR)
endif

check: all
	make -C test check

install preinstall package: base
	mkdir -p $(BUILD_DIR)
	cd $(BUILD_DIR) && $(CMAKE3) $(ROOT_DIR)
	make -C $(BUILD_DIR) -f Makefile $@

rpm: package
ifneq ($(BUILD_DIR),.)
	mv $(BUILD_DIR)/*.rpm ./
endif

clean:
ifeq (build, $(wildcard build))
	-make -C build clean
endif
	-make -C workflow clean
	-make -C test clean
	rm -rf $(DEFAULT_BUILD_DIR)
	rm -rf _include
	rm -rf _lib
	rm -f SRCINFO SRCNUMVER SRCVERSION
	rm -f ./*.rpm
	find . -name CMakeCache.txt | xargs rm -f
	find . -name Makefile       | xargs rm -f
	find . -name "*.cmake"      | xargs rm -f
	find . -name CMakeFiles     | xargs rm -rf

