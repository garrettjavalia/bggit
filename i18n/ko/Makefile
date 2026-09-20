BGBSPD_KO_DIR ?= ../../../bgbspd-ko
BGBSPD_BUILD_DIR ?= ../../../bgbspd
BOOK_DIR := $(abspath ../..)
FORMAT ?= all
PDF_TARGETS ?= a4_c_1,a4_bw_2
LANGUAGE ?= en
COMMON = python3 "$(BGBSPD_KO_DIR)/bin/build-guide.py" --book "$(BOOK_DIR)" --profile bggit --bgbspd "$(BGBSPD_BUILD_DIR)" --format "$(FORMAT)" --pdf-targets "$(PDF_TARGETS)" --lang "$(LANGUAGE)"
.PHONY: all docker status
all:
	$(COMMON)
docker:
	$(COMMON) --docker bgbspd-ko-builder
status:
	python3 "$(BGBSPD_KO_DIR)/bin/translation.py" status --book "$(BOOK_DIR)" --guide-id bggit
