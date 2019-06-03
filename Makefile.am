SUBDIRS = lib . tests
AM_CPPFLAGS = -I$(top_srcdir)/lib
AM_CFLAGS = $(WARN_CFLAGS) $(WERROR_CFLAGS)
ACLOCAL_AMFLAGS = -I m4

# Tell the linker to omit references to unused shared libraries.
AM_LDFLAGS = $(IGNORE_UNUSED_LIBRARIES_CFLAGS)

man_MANS = src/pigz.1

bin_PROGRAMS = pigz
pigz_LDADD = $(LIBOBJS)
pigz_SOURCES = \
	       $(man_MANS) \
	       src/pigz.c \
	       src/tailor.h \
	       src/util.c \
	       lib/try.c \
	       lib/try.h \
	       lib/yarn.c \
	       lib/yarn.h \
	       lib/zopfli/blocksplitter.c \
	       lib/zopfli/blocksplitter.h \
	       lib/zopfli/cache.c \
	       lib/zopfli/cache.h \
	       lib/zopfli/deflate.c \
	       lib/zopfli/deflate.h \
	       lib/zopfli/hash.c \
	       lib/zopfli/hash.h \
	       lib/zopfli/katajainen.c \
	       lib/zopfli/katajainen.h \
	       lib/zopfli/lz77.c \
	       lib/zopfli/lz77.h \
	       lib/zopfli/squeeze.c \
	       lib/zopfli/squeeze.h \
	       lib/zopfli/symbols.h \
	       lib/zopfli/tree.c \
	       lib/zopfli/tree.h \
	       lib/zopfli/util.c \
	       lib/zopfli/util.h \
	       lib/zopfli/zopfli.h

EXTRA_DIST = \
	     m4/gnulib-cache.m4 \
	     lib/zopfli/README \
	     lib/zopfli/COPYING \
	     lib/zopfli/CONTRIBUTORS \
	     AUTHORS \
	     ChangeLog \
	     COPYING \
	     NEWS \
	    #  pigz.pdf \
	    #  pigz.spec \
	     README \
	     README-hacking

dist_doc_DATA = README
