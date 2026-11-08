#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.68])
AC_INIT([idevicerestore], [m4_esyscmd(./git-version-gen $RELEASE_VERSION)], [https://github.com/libimobiledevice/idevicerestore/issues], [], [https://libimobiledevice.org])
AM_INIT_AUTOMAKE([dist-bzip2 no-dist-gzip])
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES])
AC_CONFIG_SRCDIR([src/])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([m4])

# Check if we have a version defined
if test -z $PACKAGE_VERSION; then
  AC_MSG_ERROR([PACKAGE_VERSION is not defined. Make sure to configure a source tree checked out from git or that .tarball-version is present.])
fi

# Minimum package versions
LIBIRECOVERY_VERSION=1.3.0
LIBIMOBILEDEVICE_VERSION=1.4.0
LIBUSBMUXD_VERSION=2.0.2
LIBPLIST_VERSION=2.6.0
LIMD_GLUE_VERSION=1.3.0
LIBTATSU_VERSION=1.0.4
LIBZIP_VERSION=1.0
LIBCURL_VERSION=7.0

AC_SUBST(LIBIRECOVERY_VERSION)
AC_SUBST(LIBIMOBILEDEVICE_VERSION)
AC_SUBST(LIBUSBMUXD_VERSION)
AC_SUBST(LIBPLIST_VERSION)
AC_SUBST(LIMD_GLUE_VERSION)
AC_SUBST(LIBTATSU_VERSION)
AC_SUBST(LIBZIP_VERSION)
AC_SUBST(LIBCURL_VERSION)

# Checks for programs.
AC_PROG_CC
AM_PROG_CC_C_O
LT_INIT

# Checks for libraries.
LIBIRECOVERY_REQUIRES_STR="libirecovery-1.0 >= $LIBIRECOVERY_VERSION"
LIBIMOBILEDEVICE_REQUIRES_STR="libimobiledevice-1.0 >= $LIBIMOBILEDEVICE_VERSION"
LIBUSBMUXD_REQUIRES_STR="libusbmuxd-2.0 >= $LIBUSBMUXD_VERSION"
LIBPLIST_REQUIRES_STR="libplist-2.0 >= $LIBPLIST_VERSION"
LIMD_GLUE_REQUIRES_STR="libimobiledevice-glue-1.0 >= $LIMD_GLUE_VERSION"
LIBTATSU_REQUIRES_STR="libtatsu-1.0 >= $LIBTATSU_VERSION"
LIBZIP_REQUIRES_STR="libzip >= $LIBZIP_VERSION"
LIBCURL_REQUIRES_STR="libcurl >= $LIBCURL_VERSION"
ZLIB_REQUIRES_STR="zlib"

PKG_CHECK_MODULES(libirecovery, $LIBIRECOVERY_REQUIRES_STR)
PKG_CHECK_MODULES(libimobiledevice, $LIBIMOBILEDEVICE_REQUIRES_STR)
PKG_CHECK_MODULES(libusbmuxd, $LIBUSBMUXD_REQUIRES_STR)
PKG_CHECK_MODULES(libplist, $LIBPLIST_REQUIRES_STR)
PKG_CHECK_MODULES(limd_glue, $LIMD_GLUE_REQUIRES_STR)
PKG_CHECK_MODULES(libtatsu, $LIBTATSU_REQUIRES_STR)
PKG_CHECK_MODULES(libzip, $LIBZIP_REQUIRES_STR)
PKG_CHECK_MODULES(libcurl, $LIBCURL_REQUIRES_STR)
PKG_CHECK_MODULES(zlib, $ZLIB_REQUIRES_STR)

# optional
AC_SUBST([libirecovery_requires], [$LIBIRECOVERY_REQUIRES_STR])
AC_SUBST([libimobiledevice_requires], [$LIBIMOBILEDEVICE_REQUIRES_STR])
AC_SUBST([libusbmuxd_requires], [$LIBUSBMUXD_REQUIRES_STR])
AC_SUBST([libplist_requires], [$LIBPLIST_REQUIRES_STR])
AC_SUBST([limd_glue_requires], [$LIMD_GLUE_REQUIRES_STR])
AC_SUBST([libtatsu_requires], [$LIBTATSU_REQUIRES_STR])
AC_SUBST([libzip_requires], [$LIBZIP_REQUIRES_STR])
AC_SUBST([libcurl_requires], [$LIBCURL_REQUIRES_STR])
AC_SUBST([zlib_requires], [$ZLIB_REQUIRES_STR])

AC_SUBST([release_version], [$VERSION])

AC_CHECK_FUNCS([strsep strcspn mkstemp realpath])
if test x$ac_cv_func_strsep != xyes; then
  if test x$ac_cv_func_strcspn != xyes; then
    AC_MSG_ERROR([You need either strsep or strcspn to build $PACKAGE])
  fi
fi

AC_SEARCH_LIBS([ceil], [m])

AC_CHECK_HEADER(endian.h, [ac_cv_have_endian_h="yes"], [ac_cv_have_endian_h="no"])
if test "x$ac_cv_have_endian_h" = "xno"; then
  AC_DEFINE(__LITTLE_ENDIAN,1234,[little endian])
  AC_DEFINE(__BIG_ENDIAN,4321,[big endian])
  AC_C_BIGENDIAN([ac_cv_c_bigendian="yes"], [ac_cv_c_bigendian="no"], [], [])
  if test "x$ac_cv_c_bigendian" = "xyes"; then
    AC_DEFINE(__BYTE_ORDER,4321,[big endian byte order])
  else
    AC_DEFINE(__BYTE_ORDER,1234,[little endian byte order])
  fi
fi

GLOBAL_CFLAGS="-Wno-multichar -O2"
AC_LDADD=""
AC_LDFLAGS=""

CFLAGS="$CFLAGS $libplist_CFLAGS"
AC_CHECK_DECL([plist_from_json], [], [AC_MSG_ERROR([libplist with JSON format support required to build $PACKAGE_NAME])], [[#include <plist/plist.h>]])

# Check for operating system
AC_MSG_CHECKING([for platform-specific build settings])
case ${host_os} in
  *mingw32*|*cygwin*)
    AC_MSG_RESULT([${host_os}])
    win32=true
    GLOBAL_CFLAGS+=" -DWIN32 -D__LITTLE_ENDIAN__=1"
    AC_LDFLAGS+=" -static-libgcc"
    ;;
  darwin*)
    AC_MSG_RESULT([${host_os}])
    AC_DEFINE([_DARWIN_BETTER_REALPATH], [1], [Use better method for realpath])
    darwin=true
    ;;
  *)
    AC_MSG_RESULT([${host_os}])
    ;;
esac
AM_CONDITIONAL(WIN32, test x$win32 = xtrue)
AM_CONDITIONAL(DARWIN, test x$darwin = xtrue)

if test x$win32 != xtrue; then
  if test "x$ac_cv_func_mkstemp" != xyes; then
    AC_CHECK_LIB(pthread, [pthread_self], [], [AC_MSG_ERROR([pthread is required to build $PACKAGE])])
  fi
fi

CACHED_CFLAGS="$CFLAGS"
CFLAGS+=" $libimobiledevice_CFLAGS"

# check if libimobiledevice has timeout errors
AC_CACHE_CHECK(for IDEVICE_E_TIMEOUT in enum idevice_error_t, ac_cv_idevice_error_has_timeout,
	AC_COMPILE_IFELSE([AC_LANG_PROGRAM([[
		#include <libimobiledevice/libimobiledevice.h>
		]], [[
			return IDEVICE_E_TIMEOUT;
		]])],[ac_cv_idevice_error_has_timeout=yes],[ac_cv_idevice_error_has_timeout=no]))
if test "$ac_cv_idevice_error_has_timeout" = "yes"; then
	AC_DEFINE(HAVE_IDEVICE_E_TIMEOUT, 1, [Define if enum idevice_error_t defines IDEVICE_E_TIMEOUT])
fi

AC_CACHE_CHECK(for RESTORE_E_RECEIVE_TIMEOUT in enum restored_error_t, ac_cv_restored_error_has_timeout,
	AC_COMPILE_IFELSE([AC_LANG_PROGRAM([[
		#include <libimobiledevice/restore.h>
		]], [[
			return RESTORE_E_RECEIVE_TIMEOUT;
		]])],[ac_cv_restored_error_has_timeout=yes],[ac_cv_restored_error_has_timeout=no]))
if test "$ac_cv_restored_error_has_timeout" = "yes"; then
	AC_DEFINE(HAVE_RESTORE_E_RECEIVE_TIMEOUT, 1, [Define if enum restored_error_t defines RESTORE_E_RECEIVE_TIMEOUT])
fi

# check if libimobiledevice has enum idevice_connection_type
AC_CACHE_CHECK(for enum idevice_connection_type, ac_cv_enum_idevice_connection_type,
	AC_COMPILE_IFELSE([AC_LANG_PROGRAM([[
		#include <libimobiledevice/libimobiledevice.h>
	]], [[
		enum idevice_connection_type conn_type = CONNECTION_USBMUXD;
	]])],[ac_cv_enum_idevice_connection_type=yes],[ac_cv_enum_idevice_connection_type=no]))
if test "$ac_cv_enum_idevice_connection_type" = "yes"; then
	AC_DEFINE(HAVE_ENUM_IDEVICE_CONNECTION_TYPE, 1, [Define if enum idevice_connection_type is available])
fi

# check if libimobiledevice has reverse proxy support
AC_CACHE_CHECK(for reverse proxy support in libimobiledevice, ac_cv_reverse_proxy,
	AC_COMPILE_IFELSE([AC_LANG_PROGRAM([[
		#include <libimobiledevice/reverse_proxy.h>
		#include <stdlib.h>
	]], [[
		reverse_proxy_client_t rp = NULL;
		reverse_proxy_client_create_with_port(NULL, &rp, REVERSE_PROXY_DEFAULT_PORT);
	]])],[ac_cv_reverse_proxy=yes],[ac_cv_reverse_proxy=no]))
if test "$ac_cv_reverse_proxy" = "yes"; then
	AC_DEFINE(HAVE_REVERSE_PROXY, 1, [Define if libimobiledevice has a reverse proxy implementation])
fi

AC_ARG_WITH([limera1n],
            [AS_HELP_STRING([--with-limera1n],
            [build with support for limera1n exploit (default is yes)])],
            [have_limera1n=$withval],
            [have_limera1n=yes])
if test "x$have_limera1n" = "xyes"; then
  AC_DEFINE(HAVE_LIMERA1N, 1, [Define if limera1n support is available])
fi
AM_CONDITIONAL([HAVE_LIMERA1N],[test "x$have_limera1n" = "xyes"])

CFLAGS="$CACHED_CFLAGS"

AC_ARG_ENABLE([asan],
            [AS_HELP_STRING([--enable-asan],
            [enable ASAN build(default is no)])],
            [asan_build=true],
            [asan_build=false])

AC_ARG_ENABLE([debug],
            [AS_HELP_STRING([--enable-debug],
            [enable debug build(default is no)])],
            [debug_build=true],
            [debug_build=false])

if test "$asan_build" = true; then
  echo "*** Note: asan build requested ***"
  debug_build=true;
  CFLAGS+=" -fsanitize=address -fno-omit-frame-pointer -fsanitize-address-use-after-scope"
fi

if test "$debug_build" = true; then
  echo "*** Note: debug build requested ***"
  CFLAGS+=" -g -O0 -Wall -Werror -Wno-format -Wno-deprecated-declarations"
fi


AC_SUBST(GLOBAL_CFLAGS)
AC_SUBST(AC_LDFLAGS)
AC_SUBST(AC_LDADD)

# check for large file support
AC_SYS_LARGEFILE

m4_ifdef([AM_SILENT_RULES],[AM_SILENT_RULES([yes])])

AC_CONFIG_FILES([
Makefile
src/Makefile
docs/Makefile
include/Makefile
libidevicerestore.pc
])
AC_OUTPUT

echo "
Configuration for $PACKAGE $VERSION:
-------------------------------------------

  Install prefix: .........: $prefix
  ASAN build: .............: $asan_build
  debug build: ............: $debug_build

  Now type 'make' to build $PACKAGE $VERSION,
  and then 'make install' for installation.
"
