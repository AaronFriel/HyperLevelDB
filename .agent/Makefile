THREADS ?= $(shell nproc)
ifeq ($(shell [ $(THREADS) -gt 8 ] && echo yes),yes)
THREADS := 8
endif

.PHONY: all clones lint HyperLevelDB

all: HyperLevelDB

clones:

lint:
	./check.sh

HyperLevelDB:
	cd .. && autoreconf -i
	cd .. && ./configure --prefix=/opt/HyperLevelDB
	$(MAKE) -C .. -j$(THREADS)
	$(MAKE) -C .. check
