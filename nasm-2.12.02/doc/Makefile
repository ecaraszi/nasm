#
# UNIX Makefile for NASM documentation
#

top_srcdir	= ..
srcdir		= .

prefix		= /usr/local
exec_prefix	= ${prefix}
bindir		= ${exec_prefix}/bin
mandir		= ${datarootdir}/man
docdir		= $(prefix)/doc/nasm
infodir		= ${datarootdir}/info
datarootdir	= ${prefix}/share

INSTALL		= /usr/bin/install -c
INSTALL_PROGRAM	= ${INSTALL}
INSTALL_DATA	= ${INSTALL} -m 644

PERL		= perl -I$(srcdir)
MAKEINFO	= makeinfo
TEXI2DVI        = texi2dvi
TEXI2IPF	= texi2ipf
IPFC		= ipfc

ACRODIST	= false		# Acrobat Distiller
PSTOPDF		= pstopdf		# BSD/MacOS X utility
PS2PDF          = false		# Part of GhostScript

SRCS		= nasmdoc.src inslist.src changes.src version.src
OUT		= info html nasmdoc.txt nasmdoc.ps nasmdoc.pdf

# exports
export srcdir
export PERL

all: $(OUT)

os2: nasm.inf

inslist.src: inslist.pl ../insns.dat
	$(PERL) $(srcdir)/inslist.pl $(srcdir)/../insns.dat

.PHONY: html
html: html/nasmdoc0.html

RDSRC = $(PERL) $(srcdir)/rdsrc.pl -I$(srcdir)/

html/nasmdoc0.html: $(SRCS) rdsrc.pl
	mkdir -p html
	$(RDSRC) html "$<"
	mv -f *.html html

nasmdoc.dip: $(SRCS) rdsrc.pl
	$(RDSRC) dip "$<"

nasmdoc.texi: $(SRCS) rdsrc.pl
	$(RDSRC) texi "$<"

nasmdoc.txt: $(SRCS) rdsrc.pl
	$(RDSRC) txt "$<"

version.src: $(top_srcdir)/version.pl $(top_srcdir)/version
	$(PERL) $(top_srcdir)/version.pl docsrc \
		< $(top_srcdir)/version > version.src

nasmdoc.ps: nasmdoc.dip nasmlogo.eps \
	    genps.pl psfonts.ph pswidth.ph head.ps 
	$(PERL) $(srcdir)/genps.pl -headps $(srcdir)/head.ps nasmdoc.dip \
		> nasmdoc.ps

nasmdoc.pdf: nasmdoc.ps
	$(ACRODIST) -n -q --nosecurity -o $@ $< || \
	$(PS2PDF) $< $@ || \
	$(PSTOPDF) $< -o $@ 

.PHONY: info
info: info/nasm.info

info/nasm.info: nasmdoc.texi
	mkdir -p info
	$(MAKEINFO) $<
	mv -f *.info *.info-* info

# DVI output from texinfo (optional)
nasmdoc.dvi: nasmdoc.texi
	$(TEXI2DVI) nasmdoc.texi

# Rules for building an OS/2 book
nasmdoc.ipf: nasmdoc.texi
	$(TEXI2IPF) $< >$@

nasm.inf: nasmdoc.ipf
	$(IPFC) -i -s $< $@

clean:
	-rm -f *.rtf *.hpj *.texi *.gid *.ipf *.dip
	-rm -f *.aux *.cp *.fn *.ky *.pg *.log *.toc *.tp *.vr
	-rm -f inslist.src version.src

spotless: clean
	-rm -rf html info
	-rm -f *.hlp nasmdoc.txt *.inf *.pdf *.dvi
	-rm -f nasmdoc*.ps

install: all
	mkdir -p $(INSTALLROOT)$(infodir)
	$(INSTALL_DATA) info/* $(INSTALLROOT)$(infodir)
	mkdir -p $(INSTALLROOT)$(docdir)/html
	$(INSTALL_DATA) html/* $(INSTALLROOT)$(docdir)/html
	$(INSTALL_DATA) nasmdoc.ps nasmdoc.pdf nasmdoc.txt $(INSTALLROOT)$(docdir)
