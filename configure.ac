AC_PREREQ([2.65])
AC_INIT([pigz], m4_esyscmd_s([git describe --dirty]))
AC_CONFIG_SRCDIR(src/pigz.c)
AC_CONFIG_LIBOBJ_DIR([lib])
AC_CONFIG_MACRO_DIR([m4])

AH_TEMPLATE([NOTHREAD],
	    [Disable support for multithreading.])
AH_TEMPLATE([NOZOPFLI],
	    [Disable support for Google zopfli, a very effective but very
	     slow compression algorithm.])
AH_TEMPLATE([PIGZ_DEBUG],
	    [Advanced debugging info such as memory use and very detailed logging.])
AH_TEMPLATE([_FORTIFY_SOURCE],
	    [Enable glibc source fortification (gcc only).])
AH_TEMPLATE([STATIC_ZLIB],
	    [If this program statically links to zlib included with the distribution.])
AC_ARG_ENABLE([zopfli],
	[AS_HELP_STRING([--disable-zopfli],
		[Disable support for zopfli, a very slow but very effective algorithm])],
	[AS_IF([test "x$enable_zopfli" == "xno"],
	       AC_DEFINE([NOZOPFLI]))])
dnl before you ask, I don't know why they would do the x thing. it's in the
dnl autoconf manual and I don't like to think about the many idiosyncrasies of sh.

AC_ARG_ENABLE([threads],
	[AS_HELP_STRING([--disable-threads],
		[Disable speedup by multithreading.])],
	[AS_IF([test "x$enable_threads" == "xno"],
		AC_DEFINE([NOTHREAD])
	       )])

AC_ARG_ENABLE([advanced-debug],
	[AS_HELP_STRING([--enable-advanced-debug],
		[Enable advanced verbose debugging.])],
	[AS_IF([test "x$enable_advanced_debug" != "xno"],
	       AC_DEFINE([PIGZ_DEBUG]))])
AC_ARG_WITH([static-zlib],
	[AS_HELP_STRING([--with-static-zlib],
		[Use statically-linked zlib instead of linking to system zlib. (default no)])],
	[AS_IF([test "x$with_static_zlib" == "xyes"],
	       AC_CACHE_VAL([gzip_cv_lib_zlib_static], [gzip_cv_lib_zlib_static=yes]))])

AC_CONFIG_FILES([Makefile lib/Makefile tests/Makefile])



AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_HEADERS([lib/config.h:lib/config.hin])
AM_INIT_AUTOMAKE([1.11 foreign subdir-objects -Wall -Werror])
dnl TODO: change to 'foreign'->'gnu' after adding requisite infos.
AM_SILENT_RULES([yes])
AC_PROG_CC
AS_IF([test "x$GCC" == "xyes"],
       AC_DEFINE([_FORTIFY_SOURCE], 2))
AC_PROG_CC_C99

gl_EARLY
AC_MSG_CHECKING([for usable zlib])
save_libs="$LIBS"
LIBS="$save_libs -lz"
AC_CACHE_VAL([gzip_cv_lib_zlib_static],
AC_RUN_IFELSE([AC_LANG_SOURCE([#include "zlib.h"
	int main()
	{
		if (ZLIB_VERNUM >= 0x1260) return 0;
		else return 1;
	}])],
	[gzip_cv_lib_zlib_static=no],
	[gzip_cv_lib_zlib_static=yes],
	[gzip_cv_lib_zlib_static=yes]))

if test "$gzip_cv_lib_zlib_static" == "yes"; then
	LIBS="$save_libs"
	AC_MSG_RESULT([static])

else
	AC_MSG_RESULT([dynamic])
fi
AM_CONDITIONAL([STATIC_ZLIB], [test "$gzip_cv_lib_zlib_static" == "yes"])
AM_CONDITIONAL([NOZOPFLI], [test "x$enable_zopfli" == "xno"])
AM_CONDITIONAL([NOTHREAD], [test "x$enable_threads" == "xno"])

dnl why would you not have this. it shouldn't compile anyway.
dnl AC_FUNC_STRNLEN
dnl AC_LANG_CONFTEST(
dnl	[AC_LANG_SOURCE(

gl_INIT
dnl ignored warnings:
nw=
nw="$nw -Wdouble-promotion"
nw="$nw -Wsuggest-attribute=format"
nw="$nw -Wsuggest-attribute=const"
nw="$nw -Wsuggest-attribute=pure"
nw="$nw -Wsuggest-attribute=noreturn"
nw="$nw -Wunused-macros"
AS_CASE([$host],
      [i?86-*-*], [nw="$nw -Wcast-align"],
      [x86_64-*-*], [nw="$nw -Wcast-align"])

gl_MANYWARN_ALL_GCC([warnings])
gl_MANYWARN_COMPLEMENT([warnings], [$warnings], [$nw])
for w in $warnings; do
	gl_WARN_ADD([$w])
done
AC_PROG_RANLIB
AC_OUTPUT
