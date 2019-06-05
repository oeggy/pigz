SUBDIRS = lib . tests
ACLOCAL_AMFLAGS = -I m4

man_MANS = src/gzip.1

bin_PROGRAMS = gzip
base_gzip_ldadd = $(LIBOBJS)
base_gzip_sources = \
	       $(man_MANS) \
	       src/gzip.c \
	       lib/try.c \
	       lib/try.h

if !NOTHREAD
thread_gzip_sources = \
	       lib/yarn.c \
	       lib/yarn.h

thread_gzip_ldadd = $(LIB_PTHREAD)
endif

if !NOZOPFLI
zopfli_gzip_sources = \
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

zopfli_gzip_ldadd = -lm
endif

if STATIC_ZLIB
zlib_gzip_sources = \
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
gzip_zlib_cppflags = -I$(top_srcdir)/lib/zlib
endif

gzip_LDADD = $(base_gzip_ldadd) $(thread_gzip_ldadd) $(zopfli_gzip_ldadd)
gzip_SOURCES = $(base_gzip_sources) $(thread_gzip_sources) $(zopfli_gzip_sources) $(zlib_gzip_sources)
AM_CPPFLAGS = -I$(top_srcdir)/lib $(gzip_zlib_cppflags)
AM_CFLAGS = $(WARN_CFLAGS) $(WERROR_CFLAGS)
# Tell the linker to omit references to unused shared libraries.
AM_LDFLAGS = $(IGNORE_UNUSED_LIBRARIES_CFLAGS)


gzip_extra_dist = \
	     src/util.c \
	     m4/gnulib-cache.m4 \
	     AUTHORS \
	     ChangeLog \
	     COPYING \
	     NEWS \
	    #  gzip.pdf \
	    #  gzip.spec \
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

EXTRA_DIST = $(gzip_extra_dist) $(zlib_extra_dist) $(zopfli_extra_dist)
dist_doc_DATA = README
clean-local:
	-git submodule foreach git clean -dfx
