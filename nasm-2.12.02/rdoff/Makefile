#
# Auto-configuring Makefile for RDOFF object file utils; part of the
# Netwide Assembler
#
# The Netwide Assembler is copyright (C) 1996 Simon Tatham and
# Julian Hall. All rights reserved. The software is
# redistributable under the license given in the file "LICENSE"
# distributed in the NASM archive.

top_srcdir	= ..
srcdir		= .

prefix		= /usr/local
exec_prefix	= ${prefix}
bindir		= ${exec_prefix}/bin
mandir		= ${datarootdir}/man
datarootdir	= ${prefix}/share

CC		= gcc
CFLAGS		= -g -O2 -W -Wall -std=c99 -fwrapv -pedantic -Werror=unknown-warning-option -Wno-long-long -Werror=implicit -Werror=missing-braces -Werror=return-type -Werror=trigraphs -Werror=pointer-arith -Werror=strict-prototypes -Werror=missing-prototypes -Werror=missing-declarations -Werror=comment -Werror=vla
BUILD_CFLAGS	= $(CFLAGS) -DHAVE_CONFIG_H
INTERNAL_CFLAGS = -I$(srcdir) -I$(top_srcdir) -I..
ALL_CFLAGS	= $(BUILD_CFLAGS) $(INTERNAL_CFLAGS)
LDFLAGS		= 

INSTALL		= /usr/bin/install -c
INSTALL_PROGRAM	= ${INSTALL}
INSTALL_DATA	= ${INSTALL} -m 644
LN_S 		= ln -s
MKDIR		= mkdir

LDRDFLIBS 	= rdoff.$(O) symtab.$(O) collectn.$(O) \
		  rdlib.$(O) segtab.$(O) hash.$(O)
RDXLIBS 	= rdoff.$(O) rdfload.$(O) symtab.$(O) collectn.$(O) hash.$(O)
NASMLIB		= ../nasmlib.$(O)

# Binary suffixes
O               = o
X               = 

PROGRAMS = rdfdump$(X) ldrdf$(X) rdx$(X) rdflib$(X) \
	   rdf2bin$(X) rdf2com$(X) rdf2ith$(X) rdf2ihx$(X) rdf2srec$(X)

.SUFFIXES: .c .i .s .$(O) .1 .man

.c.$(O):
	$(CC) -c $(ALL_CFLAGS) $<

all: $(PROGRAMS)

rdfdump$(X): rdfdump.$(O) rdoff.$(O)
	$(CC) $(LDFLAGS) -o rdfdump$(X) rdfdump.$(O) rdoff.$(O)
ldrdf$(X): ldrdf.$(O) $(LDRDFLIBS) $(NASMLIB)
	$(CC) $(LDFLAGS) -o ldrdf$(X) ldrdf.$(O) $(LDRDFLIBS) $(NASMLIB)
rdx$(X): rdx.$(O) $(RDXLIBS) $(NASMLIB)
	$(CC) $(LDFLAGS) -o rdx$(X) rdx.$(O) $(RDXLIBS) $(NASMLIB)
rdflib$(X): rdflib.$(O)
	$(CC) $(LDFLAGS) -o rdflib$(X) rdflib.$(O)
rdf2bin$(X): rdf2bin.$(O) $(RDXLIBS) $(NASMLIB)
	$(CC) $(LDFLAGS) -o rdf2bin$(X) rdf2bin.$(O) $(RDXLIBS) $(NASMLIB)
rdf2com$(X):
	rm -f rdf2com$(X) && $(LN_S) rdf2bin$(X) rdf2com$(X)
rdf2ith$(X):
	rm -f rdf2ith$(X) && $(LN_S) rdf2bin$(X) rdf2ith$(X)
rdf2ihx$(X):
	rm -f rdf2ihx$(X) && $(LN_S) rdf2bin$(X) rdf2ihx$(X)
rdf2srec$(X):
	rm -f rdf2srec$(X) && $(LN_S) rdf2bin$(X) rdf2srec$(X)

rdf2ihx.$(O): rdf2ihx.c
rdf2bin.$(O): rdf2bin.c
rdfdump.$(O): rdfdump.c
rdoff.$(O): rdoff.c rdoff.h
ldrdf.$(O): ldrdf.c rdoff.h ../nasmlib.h symtab.h collectn.h rdlib.h
symtab.$(O): symtab.c symtab.h
collectn.$(O): collectn.c collectn.h
rdx.$(O): rdx.c rdoff.h rdfload.h symtab.h
rdfload.$(O): rdfload.c rdfload.h rdoff.h collectn.h symtab.h
rdlib.$(O): rdlib.c rdlib.h
rdflib.$(O): rdflib.c
segtab.$(O): segtab.c

clean:
	rm -f *.$(O) $(PROGRAMS)

spotless: clean
	rm -f Makefile

distclean: spotless

install: all
	$(MKDIR) -p $(INSTALLROOT)$(bindir)
	$(INSTALL_PROGRAM) rdfdump$(X) $(INSTALLROOT)$(bindir)/rdfdump$(X)
	$(INSTALL_PROGRAM) ldrdf$(X) $(INSTALLROOT)$(bindir)/ldrdf$(X)
	$(INSTALL_PROGRAM) rdx$(X) $(INSTALLROOT)$(bindir)/rdx$(X)
	$(INSTALL_PROGRAM) rdflib$(X) $(INSTALLROOT)$(bindir)/rdflib$(X)
	$(INSTALL_PROGRAM) rdf2bin$(X) $(INSTALLROOT)$(bindir)/rdf2bin$(X)
	cd $(INSTALLROOT)$(bindir) && rm -f rdf2com$(X) && $(LN_S) rdf2bin$(X) rdf2com$(X)
	cd $(INSTALLROOT)$(bindir) && rm -f rdf2ith$(X) && $(LN_S) rdf2bin$(X) rdf2ith$(X)
	cd $(INSTALLROOT)$(bindir) && rm -f rdf2ihx$(X) && $(LN_S) rdf2bin$(X) rdf2ihx$(X)
	cd $(INSTALLROOT)$(bindir) && rm -f rdf2srec$(X) && $(LN_S) rdf2bin$(X) rdf2srec$(X)
	$(MKDIR) -p $(INSTALLROOT)$(mandir)/man1
	$(INSTALL_DATA) $(srcdir)/*.1 $(INSTALLROOT)$(mandir)/man1/
