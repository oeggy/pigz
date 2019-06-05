SUBDIRS = lib . tests
ACLOCAL_AMFLAGS = -I m4

man_MANS = src/pigz.1

bin_PROGRAMS = pigz
base_pigz_ldadd = $(LIBOBJS)
base_pigz_sources = \
	       $(man_MANS) \
	       src/pigz.c \
	       lib/try.c \
	       lib/try.h

if !NOTHREAD
thread_pigz_sources = \
	       lib/yarn.c \
	       lib/yarn.h

thread_pigz_ldadd = $(LIB_PTHREAD)
endif

if !NOZOPFLI
zopfli_pigz_sources = \
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

zopfli_pigz_ldadd = -lm
endif

if STATIC_ZLIB
zlib_pigz_sources = \
	lib/zlib/adler32.c \
	lib/zlib/compress.c \
	lib/zlib/crc32.h \
	lib/zlib/crc32.c \
	lib/zlib/deflate.h \
	lib/zlib/deflate.c \
	lib/zlib/gzguts.h \
	lib/zlib/gzclose.c \
	lib/zlib/gzlib.c \
	lib/zlib/gzread.c \
	lib/zlib/gzwrite.c \
	lib/zlib/infback.c \
	lib/zlib/inffast.h \
	lib/zlib/inffast.c \
	lib/zlib/inffixed.h \
	lib/zlib/inflate.h \
	lib/zlib/inflate.c \
	lib/zlib/inftrees.h \
	lib/zlib/inftrees.c \
	lib/zlib/trees.h \
	lib/zlib/trees.c \
	lib/zlib/zconf.h \
	lib/zlib/zlib.h \
	lib/zlib/zutil.h \
	lib/zlib/zutil.c \
	lib/zlib/uncompr.c
pigz_zlib_cppflags = -I$(top_srcdir)/lib/zlib
endif

pigz_LDADD = $(base_pigz_ldadd) $(thread_pigz_ldadd) $(zopfli_pigz_ldadd)
pigz_SOURCES = $(base_pigz_sources) $(thread_pigz_sources) $(zopfli_pigz_sources) $(zlib_pigz_sources)
AM_CPPFLAGS = -I$(top_srcdir)/lib $(pigz_zlib_cppflags)
AM_CFLAGS = $(WARN_CFLAGS) $(WERROR_CFLAGS)
# Tell the linker to omit references to unused shared libraries.
AM_LDFLAGS = -Wl, $(IGNORE_UNUSED_LIBRARIES_CFLAGS)


pigz_extra_dist = \
	     src/util.c \
	     m4/gnulib-cache.m4 \
	     AUTHORS \
	     ChangeLog \
	     COPYING \
	     NEWS \
	    #  pigz.pdf \
	    #  pigz.spec \
	     README \
	     README-hacking
if STATIC_ZLIB
zlib_extra_dist = \
	lib/zlib/README \
	lib/zlib/ChangeLog
endif

if !NOZOPFLI
zopfli_extra_dist = \
	lib/zopfli/README \
	lib/zopfli/CONTRIBUTORS \
	lib/zopfli/COPYING
endif

EXTRA_DIST = $(pigz_extra_dist) $(zlib_extra_dist) $(zopfli_extra_dist)
dist_doc_DATA = README
clean-local:
	-git submodule foreach git clean -dfx
