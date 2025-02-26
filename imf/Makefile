#    SPDX-FileCopyrightText: 2021 Monaco F. J. <monaco@usp.br>
#    SPDX-FileCopyrightText: 2024 vNishimura <vitor.vian@usp.br>
#   
#    SPDX-License-Identifier: GPL-3.0-or-later
#
#  This file is a derivative work from SYSeg (https://gitlab.com/monaco/syseg)
#  and contains modifications carried out by the following author(s):
#  vNishimura <vitor.vian@usp.br>

##
## Relevant rules.
## Update if necessary to solve the programming challenge.
##
libfake.so: fake_strcmp.c
	gcc --shared -fPIC fake_strcmp.c -o libfake.so -m32

decrypt: libfake.so
	@echo "pre-loading dynamic library to bypass the access token security check"
	@echo "Please wait..."
	export LD_LIBRARY_PATH=.
	LD_PRELOAD=./libfake.so ./decode secret.cry

clean: libfake.so
	rm libfake.so

.PHONY: decrypt
## End of relevant rules to solve the challenge

## 
## Rules used by SYSeg.
## Not relevant for the example.
##

EXPORT_FILES = README Makefile decode libcry.so
EXPORT_NEW_FILES = NOTEBOOK


## Export rules.
## These rules are not relevant for the exercise.
## They are used by SYSeg to create a self-contained distribution bundle
## of the source directory, suitable for exporting project files.

TARNAME=imf


syseg-export export:
	@if test -f .dist; then\
	  echo "This is an exported bundle already. Nothing to be done.";\
	  exit 0;\
	else\
	  make do_export;\
	fi

do_export:
	rm -rf $(TARNAME)
	mkdir $(TARNAME)
	for i in $(EXPORT_FILES); do\
	  ../../tools/syseg-export -f $$i $(TARNAME);\
	done
	for i in $(EXPORT_NEW_FILES); do\
	  ../../tools/syseg-newfile -c $(TARNAME)/$$i;\
	done
	make $(TARNAME)/Makefile
	echo "DOCM4_DIST=yes" > $(TARNAME)/.dist
	tar zcvf $(TARNAME).tar.gz $(TARNAME)


clean-export:
	rm -f $(TARNAME).tar.gz
	rm -rf $(TARNAME)

.PHONY: syseg-export export clean-export

## End of export rules.

## 

# Update docm4 files.
#
# These rules are not relevant for the source-code examples in this directory.
# The are provided to automatically update docm4 files when their respective
# sources change. It won't have effect in an exported directory, though.

-include .dist

ifneq (yes,$(DOCM4_DIST))


docm4_deps = ../../tools/docm4.m4 ../../tools/bintools.m4

Makefile : Makefile.m4 $(docm4_deps) 
	@path=$$(pwd) ; dir=$${path##*/};\
	 make -C .. $$dir/Makefile;
	@echo "syseg>>: Makefile has changed: all targets may need to be rebuilt."
	@echo "Building" $$(test -z "$(MAKECMDGOALS)" && echo "the default target" || echo "$(MAKECMDGOALS)")
	@make -f Makefile $(MAKECMDGOALS)

docm4_supported_files = README 

$(docm4_supported_files): % : %.m4 $(docm4_deps)
	@path=$$(pwd) ; dir=$${path##*/}; make -C .. $$dir/$(*) 

updatem4: Makefile
#	@path=$$(pwd) ; dir=$${path##*/}; make -C .. $$(for i in $(docm4_supported_files); do test -f $$dir/$$i.m4 && echo "$$dir/$$i" ; done)
	@make $(docm4_supported_files)

endif

# End of update docm4 files.


