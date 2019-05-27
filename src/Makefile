# This file is part of the book
#
#    Statistics Lecture Notes
#
# Copyright (C) 2019  Jason D. Underdown
# See the file COPYING for copying conditions.

# Required software:
#
#    install         (system tool to make directories)
#    xsltproc        (xml/xsl text processor)
#    pdflatex        (latex -> pdf (comes with any TeX installation))
#

MBXSL = ~/Dev/mathbook/xsl
MBCSS = ~/Dev/mathbook/css
MBSCRIPT = ~/Dev/mathbook/script
MBDTD = ~/Dev/mathbook/schema/dtd

SRC = ~/Dev/Books/stats/src
IMAGES = $(SRC)/images
OUTPUT = ~/Dev/Books/stats/docs

HTML = $(OUTPUT)/html
PDF  = $(OUTPUT)/pdf
DOCTEST = $(OUTPUT)/doctest

PDFVIEWER = open -a /Applications/Preview.app $1
HTMLVIEWER = open -a /Applications/Safari.app $1

.PHONY: help check
help:
	@echo "The files in this directory are used to build the book,"
	@echo
	@echo "     'Statistics Lecture Notes'"
	@echo
	@echo "The supported output formats are HTML and PDF. A few programs"
	@echo "are required in order to build the book. To check whether the"
	@echo "required programs are installed on your system you can run:"
	@echo
	@echo " make check"
	@echo
	@echo "If you have the required programs, then you can run the following"
	@echo "commands to make the HTML and PDF formats of the book."
	@echo
	@echo " make html"
	@echo " make pdf"
	@echo

check: .check_install .check_xsltproc .check_pdflatex
	@echo "You have all the required programs installed. Go ahead and run:"
	@echo
	@echo " make html"
	@echo " make pdf"
	@echo

html: .check_install .check_xsltproc
	install -d $(HTML)
	cp -a $(IMAGES) $(HTML)
	-rm $(HTML)/*.html
	cd $(HTML); \
	xsltproc -xinclude \
	        -stringparam numbering.maximum.level 3 \
		-stringparam html.knowl.example no \
		$(MBXSL)/mathbook-html.xsl $(SRC)/stats.xml
	@echo "Successfully generated HTML output."


viewhtml:
	$(HTMLVIEWER) $(HTML)/index.html &

diagrams:
	install -d $(HTML)/images
	-rm $(HTML)/images/*
	$(MBSCRIPT)/mbx -v -c latex-image -f svg -d $(HTML)/images $(SRC)/stats.xml
#	$(MBSCRIPT)/mbx -v -c sageplot    -f pdf -d $(HTML)/images $(SRC)/stats.xml
#	$(MBSCRIPT)/mbx -v -c sageplot    -f svg -d $(HTML)/images $(SRC)/stats.xml


relax:  .check_trang
	cd $(MBDTD); \
	trang mathbook.dtd $(SRC)/mathbook.rnc

rsync:  .check_rsync
	rsync -avz -e ssh $(HTML)/ xserver.math.utah.edu:~/public_html/stats/

latex:  .check_xsltproc
	install -d $(PDF)
	cp -a $(IMAGES) $(PDF)
	cd $(PDF); \
	xsltproc -xinclude -o stats.tex $(MBXSL)/mathbook-latex.xsl $(SRC)/stats.xml

pdf:    .check_pdflatex
	cd $(PDF); \
	pdflatex stats.tex; \
	pdflatex stats.tex

viewpdf:
	$(PDFVIEWER) $(PDFOUT)/stats.pdf &

# first doctest writes the file "testlist-deala.sobj" in current directory
#   this can look suspicious when left behind
doctest:
	install -d $(DOCTEST)
	-rm $(DOCTEST)/*.py
	cd $(DOCTEST); \
	xsltproc --xinclude  --stringparam chunk-level 2 $(XSL)/mathbook-sage-doctest.xsl $(SRC)/stats.xml; \
	$(SAGE) -tp  *.py

.check_xsltproc:
	@CHECKPATH=`which xsltproc`; \
	if [ "$$CHECKPATH" == "" ]; then \
		echo ""; \
		echo ""; \
		echo "You need to install 'xsltproc'"; \
		echo ""; \
		exit 1; \
	fi
	@echo "The 'xsltproc' software is installed."
	@touch .check_xsltproc

.check_trang:
	@CHECKPATH=`which trang`; \
	if [ "$$CHECKPATH" == "" ]; then \
		echo ""; \
		echo ""; \
		echo "You need to install 'trang'"; \
		echo ""; \
		exit 1; \
	fi
	@echo "The 'trang' software is installed."
	@touch .check_trang

.check_install:
	@CHECKPATH=`which install`; \
	if [ "$$CHECKPATH" == "" ]; then \
		echo ""; \
		echo ""; \
		echo "You need the 'install' program installed"; \
		echo ""; \
		exit 1; \
	fi
	@echo "The 'install' software is installed."
	@touch .check_install

.check_pdflatex:
	@CHECKPATH=`which pdflatex`; \
	if [ "$$CHECKPATH" == "" ]; then \
		echo ""; \
		echo ""; \
		echo "You need the 'pdflatex' program installed"; \
		echo ""; \
		exit 1; \
	fi
	@echo "The 'pdflatex' software is installed."
	@touch .check_pdflatex

.check_rsync:
	@CHECKPATH=`which rsync`; \
	if [ "$$CHECKPATH" == "" ]; then \
		echo ""; \
		echo ""; \
		echo "You need the 'rsync' program installed"; \
		echo ""; \
		exit 1; \
	fi
	@echo "The 'rsync' software is installed."
	@touch .check_rsync
