TITLE="Beej's Guide to Git"
SUBTITLE=""
AUTHOR='Brian “Beej Jorgensen” Hall'
VERSION_DATE="v1.2.8, Copyright © April 18, 2026"
GUIDE_ID=bggit

# Book metadata stays here; all build behavior comes from the pinned shared tool.
.DEFAULT_GOAL := all
.PHONY: all clean pristine
all:
	$(MAKE) -C .. docker
$(GUIDE_ID).html $(GUIDE_ID)-wide.html:
	$(MAKE) -C .. html
$(GUIDE_ID)_%.pdf:
	$(MAKE) -C .. pdf PDF_TARGETS=$*
clean pristine:
	$(MAKE) -C .. $@
