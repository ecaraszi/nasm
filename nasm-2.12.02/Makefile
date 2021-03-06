#
# Auto-configuring Makefile for the Netwide Assembler.
#
# The Netwide Assembler is copyright (C) 1996 Simon Tatham and
# Julian Hall. All rights reserved. The software is
# redistributable under the license given in the file "LICENSE"
# distributed in the NASM archive.

top_srcdir	= .
srcdir		= .
objdir		= .

prefix		= /usr/local
exec_prefix	= ${prefix}
bindir		= ${exec_prefix}/bin
mandir		= ${datarootdir}/man
datarootdir	= ${prefix}/share

CC		= gcc
CFLAGS		= -g -O2 -W -Wall -std=c99 -fwrapv -pedantic -Werror=unknown-warning-option -Wno-long-long -Werror=implicit -Werror=missing-braces -Werror=return-type -Werror=trigraphs -Werror=pointer-arith -Werror=strict-prototypes -Werror=missing-prototypes -Werror=missing-declarations -Werror=comment -Werror=vla
BUILD_CFLAGS	= $(CFLAGS) -DHAVE_CONFIG_H
INTERNAL_CFLAGS = -I$(srcdir) -I.
ALL_CFLAGS	= $(BUILD_CFLAGS) $(INTERNAL_CFLAGS)
LDFLAGS		= 
LIBS		= 
PERL		= perl -I$(srcdir)/perllib -I$(srcdir)

XOBJS		= 

INSTALL		= /usr/bin/install -c
INSTALL_PROGRAM	= ${INSTALL}
INSTALL_DATA	= ${INSTALL} -m 644

NROFF		= nroff
ASCIIDOC	= asciidoc
XMLTO		= false

MAKENSIS	= makensis

MKDIR		= mkdir
RM		= rm

STRIP		= strip
FIND		= find

# Binary suffixes
O               = o
X               = 

# Debug stuff
ifeq ($(TRACE),1)
	CFLAGS += -DNASM_TRACE
endif

.SUFFIXES: .c .i .s .$(O) .1 .txt .xml

.PHONY: all doc rdf install clean distclean cleaner spotless install_rdf test
.PHONY: install_doc everything install_everything strip perlreq dist tags TAGS
.PHONY: manpages nsis

.c.$(O):
	$(CC) -c $(ALL_CFLAGS) -o $@ $<

.c.s:
	$(CC) -S $(ALL_CFLAGS) -o $@ $<

.c.i:
	$(CC) -E $(ALL_CFLAGS) -o $@ $<

.txt.xml:
	$(ASCIIDOC) -b docbook -d manpage -o $@ $<

.xml.1:
	$(XMLTO) man --skip-validation $< 2>/dev/null

#-- Begin File Lists --#
NASM =	nasm.$(O) nasmlib.$(O) ver.$(O) \
	raa.$(O) saa.$(O) rbtree.$(O) srcfile.$(O) \
	realpath.$(O) \
	float.$(O) insnsa.$(O) insnsb.$(O) \
	directiv.$(O) \
	assemble.$(O) labels.$(O) hashtbl.$(O) crc64.$(O) parser.$(O) \
	output/outform.$(O) output/outlib.$(O) output/nulldbg.$(O) \
	output/nullout.$(O) \
	output/outbin.$(O) output/outaout.$(O) output/outcoff.$(O) \
	output/outelf.$(O) output/outelf32.$(O) output/outelf64.$(O) \
	output/outelfx32.$(O) \
	output/outobj.$(O) output/outas86.$(O) output/outrdf2.$(O) \
	output/outdbg.$(O) output/outieee.$(O) output/outmacho.$(O) \
	md5c.$(O) output/codeview.$(O) \
	preproc.$(O) quote.$(O) pptok.$(O) \
	macros.$(O) listing.$(O) eval.$(O) exprlib.$(O) stdscan.$(O) \
	strfunc.$(O) tokhash.$(O) regvals.$(O) regflags.$(O) \
	ilog2.$(O) \
	lib/strlcpy.$(O) \
	preproc-nop.$(O) \
	disp8.$(O) \
	iflag.$(O)

NDISASM = ndisasm.$(O) disasm.$(O) sync.$(O) nasmlib.$(O) ver.$(O) \
	insnsd.$(O) insnsb.$(O) insnsn.$(O) regs.$(O) regdis.$(O) \
	disp8.$(O) iflag.$(O)
#-- End File Lists --#

all: nasm$(X) ndisasm$(X) rdf

nasm$(X): $(NASM) $(XOBJS)
	$(CC) $(LDFLAGS) -o nasm$(X) $(NASM) $(XOBJS) $(LIBS)

ndisasm$(X): $(NDISASM) $(XOBJS)
	$(CC) $(LDFLAGS) -o ndisasm$(X) $(NDISASM) $(XOBJS) $(LIBS)

# These source files are automagically generated from a single
# instruction-table file by a Perl script. They're distributed,
# though, so it isn't necessary to have Perl just to recompile NASM
# from the distribution.
INSDEP = insns.dat insns.pl

iflag.c: $(INSDEP)
	$(PERL) $(srcdir)/insns.pl -fc $(srcdir)/insns.dat
iflaggen.h: $(INSDEP)
	$(PERL) $(srcdir)/insns.pl -fh $(srcdir)/insns.dat
insnsb.c: $(INSDEP)
	$(PERL) $(srcdir)/insns.pl -b $(srcdir)/insns.dat
insnsa.c: $(INSDEP)
	$(PERL) $(srcdir)/insns.pl -a $(srcdir)/insns.dat
insnsd.c: $(INSDEP)
	$(PERL) $(srcdir)/insns.pl -d $(srcdir)/insns.dat
insnsi.h: $(INSDEP)
	$(PERL) $(srcdir)/insns.pl -i $(srcdir)/insns.dat
insnsn.c: $(INSDEP)
	$(PERL) $(srcdir)/insns.pl -n $(srcdir)/insns.dat

# These files contains all the standard macros that are derived from
# the version number.
version.h: version version.pl
	$(PERL) $(srcdir)/version.pl h < $(srcdir)/version > version.h
version.mac: version version.pl
	$(PERL) $(srcdir)/version.pl mac < $(srcdir)/version > version.mac
version.sed: version version.pl
	$(PERL) $(srcdir)/version.pl sed < $(srcdir)/version > version.sed
version.mak: version version.pl
	$(PERL) $(srcdir)/version.pl make < $(srcdir)/version > version.mak
nsis/version.nsh: version version.pl
	$(PERL) $(srcdir)/version.pl nsis < $(srcdir)/version > nsis/version.nsh

# This source file is generated from the standard macros file
# `standard.mac' by another Perl script. Again, it's part of the
# standard distribution.
macros.c: macros.pl pptok.ph standard.mac version.mac \
	$(srcdir)/macros/*.mac $(srcdir)/output/*.mac
	$(PERL) $(srcdir)/macros.pl $(srcdir)/standard.mac version.mac \
		$(srcdir)/macros/*.mac $(srcdir)/output/*.mac

# These source files are generated from regs.dat by yet another
# perl script.
regs.c: regs.dat regs.pl
	$(PERL) $(srcdir)/regs.pl c $(srcdir)/regs.dat > regs.c
regflags.c: regs.dat regs.pl
	$(PERL) $(srcdir)/regs.pl fc $(srcdir)/regs.dat > regflags.c
regdis.c: regs.dat regs.pl
	$(PERL) $(srcdir)/regs.pl dc $(srcdir)/regs.dat > regdis.c
regdis.h: regs.dat regs.pl
	$(PERL) $(srcdir)/regs.pl dh $(srcdir)/regs.dat > regdis.h
regvals.c: regs.dat regs.pl
	$(PERL) $(srcdir)/regs.pl vc $(srcdir)/regs.dat > regvals.c
regs.h: regs.dat regs.pl
	$(PERL) $(srcdir)/regs.pl h $(srcdir)/regs.dat > regs.h

# Assembler token hash
tokhash.c: insns.dat regs.dat tokens.dat tokhash.pl perllib/phash.ph
	$(PERL) $(srcdir)/tokhash.pl c $(srcdir)/insns.dat $(srcdir)/regs.dat \
		$(srcdir)/tokens.dat > tokhash.c

# Assembler token metadata
tokens.h: insns.dat regs.dat tokens.dat tokhash.pl perllib/phash.ph
	$(PERL) $(srcdir)/tokhash.pl h $(srcdir)/insns.dat $(srcdir)/regs.dat \
		$(srcdir)/tokens.dat > tokens.h

# Preprocessor token hash
pptok.h: pptok.dat pptok.pl perllib/phash.ph
	$(PERL) $(srcdir)/pptok.pl h $(srcdir)/pptok.dat pptok.h
pptok.c: pptok.dat pptok.pl perllib/phash.ph
	$(PERL) $(srcdir)/pptok.pl c $(srcdir)/pptok.dat pptok.c
pptok.ph: pptok.dat pptok.pl perllib/phash.ph
	$(PERL) $(srcdir)/pptok.pl ph $(srcdir)/pptok.dat pptok.ph

# Directives hash
directiv.h: directiv.dat directiv.pl perllib/phash.ph
	$(PERL) $(srcdir)/directiv.pl h $(srcdir)/directiv.dat directiv.h
directiv.c: directiv.dat directiv.pl perllib/phash.ph
	$(PERL) $(srcdir)/directiv.pl c $(srcdir)/directiv.dat directiv.c

# This target generates all files that require perl.
# This allows easier generation of distribution (see dist target).
PERLREQ = macros.c insnsb.c insnsa.c insnsd.c insnsi.h insnsn.c \
	  regs.c regs.h regflags.c regdis.c regdis.h regvals.c \
	  tokhash.c tokens.h pptok.h pptok.c pptok.ph \
	  directiv.c directiv.h \
	  iflag.c iflaggen.h \
	  version.h version.mac version.mak nsis/version.nsh
perlreq: $(PERLREQ)

# NSIS is not built except by explicit request, as it only applies to
# Windows platforms
nsis/arch.nsh: nsis/getpearch.pl nasm$(X)
	$(PERL) $(srcdir)/nsis/getpearch.pl nasm$(X) > nsis/arch.nsh

# Should only be done after "make everything".
# The use of redirection here keeps makensis from moving the cwd to the
# source directory.
nsis: nsis/nasm.nsi nsis/arch.nsh nsis/version.nsh
	$(MAKENSIS) -Dsrcdir="$(srcdir)" -Dobjdir="$(objdir)" - < "$<"

# Generated manpages, also pregenerated for distribution
manpages: nasm.1 ndisasm.1

install: nasm$(X) ndisasm$(X)
	$(MKDIR) -p $(INSTALLROOT)$(bindir)
	$(INSTALL_PROGRAM) nasm$(X) $(INSTALLROOT)$(bindir)/nasm$(X)
	$(INSTALL_PROGRAM) ndisasm$(X) $(INSTALLROOT)$(bindir)/ndisasm$(X)
	$(MKDIR) -p $(INSTALLROOT)$(mandir)/man1
	$(INSTALL_DATA) $(srcdir)/nasm.1 $(INSTALLROOT)$(mandir)/man1/nasm.1
	$(INSTALL_DATA) $(srcdir)/ndisasm.1 $(INSTALLROOT)$(mandir)/man1/ndisasm.1

clean:
	$(RM) -f *.$(O) *.s *.i
	$(RM) -f lib/*.$(O) lib/*.s lib/*.i
	$(RM) -f output/*.$(O) output/*.s output/*.i
	$(RM) -f nasm$(X) ndisasm$(X)
	$(RM) -f nasm-*-installer-*.exe
	$(RM) -f tags TAGS
	$(RM) -f nsis/arch.nsh
	cd rdoff && $(MAKE) clean

distclean: clean
	$(RM) -f config.h config.log config.status
	$(RM) -f Makefile *~ *.bak *.lst *.bin
	$(RM) -f output/*~ output/*.bak
	$(RM) -f test/*.lst test/*.bin test/*.$(O) test/*.bin
	$(RM) -rf autom4te*.cache
	cd rdoff && $(MAKE) distclean

cleaner: clean
	$(RM) -f $(PERLREQ) *.1 nasm.spec
	cd doc && $(MAKE) clean

spotless: distclean cleaner
	$(RM) -f doc/Makefile doc/*~ doc/*.bak

strip:
	$(STRIP) --strip-unneeded nasm$(X) ndisasm$(X)

rdf: nasmlib.$(O)
	cd rdoff && $(MAKE) all

TAGS:
	$(RM) -f TAGS
	$(FIND) . -name '*.[hcS]' -print | xargs etags -a

tags:
	$(RM) -f tags
	$(FIND) . -name '*.[hcS]' -print | xargs ctags -a

cscope:
	$(RM) -f cscope.out cscope.files
	$(FIND) . -name '*.[hcS]' -print > cscope.files
	cscope -b -f cscope.out

rdf_install install_rdf:
	cd rdoff && $(MAKE) install

doc:
	cd doc && $(MAKE) all

doc_install install_doc:
	cd doc && $(MAKE) install

everything: all manpages doc rdf

install_everything: everything install install_doc install_rdf

dist:
	$(MAKE) alldeps
	$(MAKE) spotless perlreq manpages spec
	autoheader
	autoconf
	$(RM) -rf ./autom4te*.cache

tar: dist
	tar -cvj --exclude CVS -C .. -f ../nasm-`cat version`-`date +%Y%m%d`.tar.bz2 `basename \`pwd\``

spec: nasm.spec

nasm.spec: nasm.spec.in version.sed
	sed -f version.sed < nasm.spec.in > nasm.spec

splint:
	splint -weak *.c

test: nasm$(X)
	cd test && $(PERL) performtest.pl --nasm=../nasm *.asm

golden: nasm$(X)
	cd test && $(PERL) performtest.pl --golden --nasm=../nasm *.asm

#
# This build dependencies in *ALL* makefiles.  Partially for that reason,
# it's expected to be invoked manually.
#
alldeps: perlreq
	$(PERL) syncfiles.pl Makefile.in Mkfiles/*.mak
	$(PERL) mkdep.pl -M Makefile.in Mkfiles/*.mak -- \
		. output lib
	./config.status

#-- Magic hints to mkdep.pl --#
# @object-ending: ".$(O)"
# @path-separator: "/"
#-- Everything below is generated by mkdep.pl - do not edit --#
assemble.$(O): assemble.c assemble.h compiler.h config.h directiv.h disp8.h \
 iflag.h iflaggen.h insns.h insnsi.h listing.h nasm.h nasmlib.h opflags.h \
 pptok.h preproc.h regs.h tables.h tokens.h
crc64.$(O): crc64.c compiler.h config.h hashtbl.h nasmlib.h
directiv.$(O): directiv.c compiler.h config.h directiv.h hashtbl.h insnsi.h \
 nasm.h nasmlib.h opflags.h pptok.h preproc.h regs.h tables.h
disasm.$(O): disasm.c compiler.h config.h directiv.h disasm.h disp8.h \
 iflag.h iflaggen.h insns.h insnsi.h nasm.h nasmlib.h opflags.h pptok.h \
 preproc.h regdis.h regs.h sync.h tables.h tokens.h
disp8.$(O): disp8.c compiler.h config.h directiv.h disp8.h insnsi.h nasm.h \
 nasmlib.h opflags.h pptok.h preproc.h regs.h tables.h
eval.$(O): eval.c compiler.h config.h directiv.h eval.h float.h insnsi.h \
 labels.h nasm.h nasmlib.h opflags.h pptok.h preproc.h regs.h tables.h
exprlib.$(O): exprlib.c compiler.h config.h directiv.h insnsi.h nasm.h \
 nasmlib.h opflags.h pptok.h preproc.h regs.h tables.h
float.$(O): float.c compiler.h config.h directiv.h float.h insnsi.h nasm.h \
 nasmlib.h opflags.h pptok.h preproc.h regs.h tables.h
hashtbl.$(O): hashtbl.c compiler.h config.h directiv.h hashtbl.h insnsi.h \
 nasm.h nasmlib.h opflags.h pptok.h preproc.h regs.h tables.h
iflag.$(O): iflag.c compiler.h config.h iflag.h iflaggen.h
ilog2.$(O): ilog2.c compiler.h config.h nasmlib.h
insnsa.$(O): insnsa.c compiler.h config.h directiv.h iflag.h iflaggen.h \
 insns.h insnsi.h nasm.h nasmlib.h opflags.h pptok.h preproc.h regs.h \
 tables.h tokens.h
insnsb.$(O): insnsb.c compiler.h config.h directiv.h iflag.h iflaggen.h \
 insns.h insnsi.h nasm.h nasmlib.h opflags.h pptok.h preproc.h regs.h \
 tables.h tokens.h
insnsd.$(O): insnsd.c compiler.h config.h directiv.h iflag.h iflaggen.h \
 insns.h insnsi.h nasm.h nasmlib.h opflags.h pptok.h preproc.h regs.h \
 tables.h tokens.h
insnsn.$(O): insnsn.c compiler.h config.h insnsi.h tables.h
labels.$(O): labels.c compiler.h config.h directiv.h hashtbl.h insnsi.h \
 labels.h nasm.h nasmlib.h opflags.h pptok.h preproc.h regs.h tables.h
lib/snprintf.$(O): lib/snprintf.c compiler.h config.h nasmlib.h
lib/strlcpy.$(O): lib/strlcpy.c compiler.h config.h
lib/vsnprintf.$(O): lib/vsnprintf.c compiler.h config.h nasmlib.h
listing.$(O): listing.c compiler.h config.h directiv.h insnsi.h listing.h \
 nasm.h nasmlib.h opflags.h pptok.h preproc.h regs.h tables.h
macros.$(O): macros.c compiler.h config.h directiv.h hashtbl.h insnsi.h \
 nasm.h nasmlib.h opflags.h output/outform.h pptok.h preproc.h regs.h \
 tables.h
md5c.$(O): md5c.c compiler.h config.h md5.h
nasm.$(O): nasm.c assemble.h compiler.h config.h directiv.h eval.h float.h \
 iflag.h iflaggen.h insns.h insnsi.h labels.h listing.h nasm.h nasmlib.h \
 opflags.h output/outform.h parser.h pptok.h preproc.h raa.h regs.h saa.h \
 stdscan.h tables.h tokens.h
nasmlib.$(O): nasmlib.c compiler.h config.h directiv.h iflag.h iflaggen.h \
 insns.h insnsi.h nasm.h nasmlib.h opflags.h pptok.h preproc.h regs.h \
 tables.h tokens.h
ndisasm.$(O): ndisasm.c compiler.h config.h directiv.h disasm.h iflag.h \
 iflaggen.h insns.h insnsi.h nasm.h nasmlib.h opflags.h pptok.h preproc.h \
 regs.h sync.h tables.h tokens.h
output/codeview.$(O): output/codeview.c compiler.h config.h directiv.h \
 hashtbl.h insnsi.h md5.h nasm.h nasmlib.h opflags.h output/outlib.h \
 output/pecoff.h pptok.h preproc.h regs.h saa.h tables.h version.h
output/nulldbg.$(O): output/nulldbg.c compiler.h config.h directiv.h \
 insnsi.h nasm.h nasmlib.h opflags.h output/outlib.h pptok.h preproc.h \
 regs.h tables.h
output/nullout.$(O): output/nullout.c compiler.h config.h directiv.h \
 insnsi.h nasm.h nasmlib.h opflags.h output/outlib.h pptok.h preproc.h \
 regs.h tables.h
output/outaout.$(O): output/outaout.c compiler.h config.h directiv.h eval.h \
 insnsi.h nasm.h nasmlib.h opflags.h output/outform.h output/outlib.h \
 pptok.h preproc.h raa.h regs.h saa.h stdscan.h tables.h
output/outas86.$(O): output/outas86.c compiler.h config.h directiv.h \
 insnsi.h nasm.h nasmlib.h opflags.h output/outform.h output/outlib.h \
 pptok.h preproc.h raa.h regs.h saa.h tables.h
output/outbin.$(O): output/outbin.c compiler.h config.h directiv.h eval.h \
 insnsi.h labels.h nasm.h nasmlib.h opflags.h output/outform.h \
 output/outlib.h pptok.h preproc.h regs.h saa.h stdscan.h tables.h
output/outcoff.$(O): output/outcoff.c compiler.h config.h directiv.h eval.h \
 insnsi.h nasm.h nasmlib.h opflags.h output/outform.h output/outlib.h \
 output/pecoff.h pptok.h preproc.h raa.h regs.h saa.h tables.h
output/outdbg.$(O): output/outdbg.c compiler.h config.h directiv.h insnsi.h \
 nasm.h nasmlib.h opflags.h output/outform.h pptok.h preproc.h regs.h \
 tables.h
output/outelf.$(O): output/outelf.c compiler.h config.h directiv.h insnsi.h \
 nasm.h nasmlib.h opflags.h output/dwarf.h output/elf.h output/outelf.h \
 output/outform.h pptok.h preproc.h rbtree.h regs.h saa.h tables.h
output/outelf32.$(O): output/outelf32.c compiler.h config.h directiv.h \
 eval.h insnsi.h nasm.h nasmlib.h opflags.h output/dwarf.h output/elf.h \
 output/outelf.h output/outform.h output/outlib.h output/stabs.h pptok.h \
 preproc.h raa.h rbtree.h regs.h saa.h stdscan.h tables.h
output/outelf64.$(O): output/outelf64.c compiler.h config.h directiv.h \
 eval.h insnsi.h nasm.h nasmlib.h opflags.h output/dwarf.h output/elf.h \
 output/outelf.h output/outform.h output/outlib.h output/stabs.h pptok.h \
 preproc.h raa.h rbtree.h regs.h saa.h stdscan.h tables.h
output/outelfx32.$(O): output/outelfx32.c compiler.h config.h directiv.h \
 eval.h insnsi.h nasm.h nasmlib.h opflags.h output/dwarf.h output/elf.h \
 output/outelf.h output/outform.h output/outlib.h output/stabs.h pptok.h \
 preproc.h raa.h rbtree.h regs.h saa.h stdscan.h tables.h
output/outform.$(O): output/outform.c compiler.h config.h directiv.h \
 insnsi.h nasm.h nasmlib.h opflags.h output/outform.h pptok.h preproc.h \
 regs.h tables.h
output/outieee.$(O): output/outieee.c compiler.h config.h directiv.h \
 insnsi.h nasm.h nasmlib.h opflags.h output/outform.h output/outlib.h \
 pptok.h preproc.h regs.h tables.h
output/outlib.$(O): output/outlib.c compiler.h config.h directiv.h insnsi.h \
 nasm.h nasmlib.h opflags.h output/outlib.h pptok.h preproc.h regs.h \
 tables.h
output/outmacho.$(O): output/outmacho.c compiler.h config.h directiv.h \
 insnsi.h nasm.h nasmlib.h opflags.h output/outform.h output/outlib.h \
 pptok.h preproc.h raa.h rbtree.h regs.h saa.h tables.h
output/outobj.$(O): output/outobj.c compiler.h config.h directiv.h eval.h \
 insnsi.h nasm.h nasmlib.h opflags.h output/outform.h output/outlib.h \
 pptok.h preproc.h regs.h stdscan.h tables.h
output/outrdf2.$(O): output/outrdf2.c compiler.h config.h directiv.h \
 insnsi.h nasm.h nasmlib.h opflags.h output/outform.h output/outlib.h \
 pptok.h preproc.h rdoff/rdoff.h regs.h saa.h tables.h
parser.$(O): parser.c compiler.h config.h directiv.h eval.h float.h iflag.h \
 iflaggen.h insns.h insnsi.h nasm.h nasmlib.h opflags.h parser.h pptok.h \
 preproc.h regs.h stdscan.h tables.h tokens.h
pptok.$(O): pptok.c compiler.h config.h hashtbl.h nasmlib.h pptok.h \
 preproc.h
preproc-nop.$(O): preproc-nop.c compiler.h config.h directiv.h insnsi.h \
 listing.h nasm.h nasmlib.h opflags.h pptok.h preproc.h regs.h tables.h
preproc.$(O): preproc.c compiler.h config.h directiv.h eval.h hashtbl.h \
 insnsi.h listing.h nasm.h nasmlib.h opflags.h pptok.h preproc.h quote.h \
 regs.h stdscan.h tables.h tokens.h
quote.$(O): quote.c compiler.h config.h nasmlib.h quote.h
raa.$(O): raa.c compiler.h config.h nasmlib.h raa.h
rbtree.$(O): rbtree.c compiler.h config.h rbtree.h
realpath.$(O): realpath.c compiler.h config.h nasmlib.h
regdis.$(O): regdis.c regdis.h regs.h
regflags.$(O): regflags.c compiler.h config.h directiv.h insnsi.h nasm.h \
 nasmlib.h opflags.h pptok.h preproc.h regs.h tables.h
regs.$(O): regs.c compiler.h config.h insnsi.h tables.h
regvals.$(O): regvals.c compiler.h config.h insnsi.h tables.h
saa.$(O): saa.c compiler.h config.h nasmlib.h saa.h
srcfile.$(O): srcfile.c compiler.h config.h hashtbl.h nasmlib.h
stdscan.$(O): stdscan.c compiler.h config.h directiv.h iflag.h iflaggen.h \
 insns.h insnsi.h nasm.h nasmlib.h opflags.h pptok.h preproc.h quote.h \
 regs.h stdscan.h tables.h tokens.h
strfunc.$(O): strfunc.c compiler.h config.h directiv.h insnsi.h nasm.h \
 nasmlib.h opflags.h pptok.h preproc.h regs.h tables.h
sync.$(O): sync.c compiler.h config.h nasmlib.h sync.h
tokhash.$(O): tokhash.c compiler.h config.h directiv.h hashtbl.h iflag.h \
 iflaggen.h insns.h insnsi.h nasm.h nasmlib.h opflags.h pptok.h preproc.h \
 regs.h stdscan.h tables.h tokens.h
ver.$(O): ver.c compiler.h config.h directiv.h insnsi.h nasm.h nasmlib.h \
 opflags.h pptok.h preproc.h regs.h tables.h version.h
