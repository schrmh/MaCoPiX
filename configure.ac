dnl Process this file with autoconf to produce a configure script.
AC_PREREQ(2.59)
AC_INIT(src/main.c)
PACKAGE=macopix

dnl version number
MAJOR_VERSION=3
MINOR_VERSION=4
MICRO_VERSION=0
BUILD_REVISION=0
INTERFACE_AGE=0
BINARY_AGE=0
EXTRA_VERSION=
VERSION=$MAJOR_VERSION.$MINOR_VERSION.$MICRO_VERSION$EXTRA_VERSION


dnl set $target
AC_CANONICAL_SYSTEM

AM_INIT_AUTOMAKE($PACKAGE, $VERSION, no-define)
dnl AC_DEFINE_UNQUOTED(PACKAGE, "$PACKAGE")
dnl AC_DEFINE_UNQUOTED(VERSION, "$VERSION")
AC_SUBST(PACKAGE)
AC_SUBST(VERSION)
AC_SUBST(MAJOR_VERSION)
AC_SUBST(MINOR_VERSION)
AC_SUBST(MICRO_VERSION)
AC_SUBST(BUILD_REVISION)


dnl libtool versioning
LT_RELEASE=$MAJOR_VERSION.$MINOR_VERSION
LT_CURRENT=`expr $MICRO_VERSION - $INTERFACE_AGE`
LT_REVISION=$INTERFACE_AGE
LT_AGE=`expr $BINARY_AGE - $INTERFACE_AGE`
AC_SUBST(LT_RELEASE)
AC_SUBST(LT_CURRENT)
AC_SUBST(LT_REVISION)
AC_SUBST(LT_AGE)

dnl Specify a header configuration file
AM_CONFIG_HEADER(config.h)

AM_MAINTAINER_MODE

dnl Checks for programs.
dnl AC_ARG_PROGRAM
AC_PROG_CC
AC_ISC_POSIX
AM_PROG_CC_STDC
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_MAKE_SET
AC_PROG_CPP
dnl AC_PROG_RANLIB
AM_PROG_LEX
AC_PROG_YACC
AM_PROG_LIBTOOL

AC_CHECK_PROG(WINDRES, windres, windres)
AM_CONDITIONAL(HAVE_WINDRES, test x"$WINDRES" != x)

# Checks for header files.
AC_FUNC_ALLOCA
AC_HEADER_DIRENT
AC_HEADER_STDC
AC_HEADER_SYS_WAIT
AC_CHECK_HEADERS([argz.h fcntl.h inttypes.h langinfo.h libintl.h limits.h locale.h malloc.h netdb.h netinet/in.h stddef.h stdint.h stdio_ext.h stdlib.h string.h strings.h sys/param.h sys/socket.h sys/utsname.h unistd.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_HEADER_STDBOOL
AC_C_CONST
AC_C_INLINE
AC_TYPE_MODE_T
AC_TYPE_OFF_T
AC_TYPE_PID_T
AC_TYPE_SIZE_T
AC_STRUCT_TM
AC_C_VOLATILE

# Checks for library functions.
AC_FUNC_CLOSEDIR_VOID
AC_FUNC_FORK
AC_FUNC_LSTAT
AC_FUNC_LSTAT_FOLLOWS_SLASHED_SYMLINK
AC_FUNC_MALLOC
AC_FUNC_MMAP
AC_FUNC_REALLOC
AC_TYPE_SIGNAL
AC_FUNC_STAT
AC_CHECK_FUNCS([__argz_count __argz_next __argz_stringify dup2 getcwd gethostbyname mempcpy memset mkdir munmap nl_langinfo setlocale socket stpcpy strcasecmp strchr strcspn strdup strrchr strstr strtol strtoul isascii memmove strncasecmp])

os_osx=no

dnl Win32
AC_ARG_WITH(win32,
		 AC_HELP_STRING([--with-win32],
		            [Make for Win32]),
			    [use_win32=yes])
if test x"${use_win32}" = "xyes"; then
AC_DEFINE(USE_WIN32,[], [Make for Win32])
AC_DEFINE(USE_BIFF,[], [Use Biff function])
CFLAGS="$CFLAGS -mms-bitfields"
LDFLAGS="$LDFLAGS -mwindows"
WINMM_LIBS="-lwinmm"
WINSOCK_LIBS="-lws2_32"
OSX_CADD=""
OSX_LDADD=""
else

dnl macOS or X11
case "$target" in
*-darwin*)
	os_osx=yes
        AC_DEFINE([USE_OSX],[],[Use Mac OS X])
dnl	CFLAGS="$CFLAGS -no-cpp-precomp -fno-common"
	;;
esac

if test x"${os_osx}" = "xyes"; then
        OSX_CADD="-x objective-c -std=c99"
        OSX_LDADD="-headerpad_max_install_names -framework carbon -framework foundation -framework cocoa -x objective-c -std=c99"
else
        OSX_CADD=""
        OSX_LDADD=""
AC_SEARCH_LIBS([XFree], [X11], [], [
  LDFLAGS="$LDFLAGS -L/usr/X11R6/lib -L/usr/X11R6/lib64"
  AC_MSG_CHECKING(["Looking for XFree in /usr/X11R6"])
  AC_SEARCH_LIBS([XFree], [X11], [], [
    AC_MSG_FAILURE(["Cannot find X11. Try setting LDFLAGS -L"], [-1])
  ])
])
fi
AC_DEFINE(USE_BIFF,[], [Use Biff function])
AC_DEFINE(USE_SOCKMSG,[], [Use Socket Message function])
AC_DEFINE(USE_COMMON,[], [Use Common Directory])
AC_DEFINE(USE_GTAR,[], [Use Gnu tar])
WINMM_LIBS=""
WINSOCK_LIBS=""
fi
AC_DEFINE(USE_SSL,[], [Use SSL for POP])
AC_SUBST(WINMM_LIBS)
AC_SUBST(WINSOCK_LIBS)
AC_SUBST(OSX_CADD)
AC_SUBST(OSX_LDADD)

dnl UNLHA32
AC_ARG_WITH(unlha32,
		 AC_HELP_STRING([--with-unlha32],
		            [Make with UNLHA32 for Win32]),
			    [use_unlha32=yes])
if test x"${use_unlha32}" = "xyes"; then
AC_DEFINE(USE_UNLHA32,[], [Make with UNLHA32 for Win32])
UNLHA32_LIBS="-L. -lunlha32"
else
UNLHA32_LIBS=""
fi
AC_SUBST(UNLHA32_LIBS)

dnl TAR64/32
AC_ARG_WITH(tar64,
		 AC_HELP_STRING([--with-tar64],
		            [Make with TAR64 for Win32]),
			    [use_tar64=yes])
if test x"${use_tar64}" = "xyes"; then
AC_DEFINE(USE_TAR32,[], [Make with TAR32 for Win32])
TAR32_LIBS="-L. -ltar64"
else
AC_ARG_WITH(tar32,
		 AC_HELP_STRING([--with-tar32],
		            [Make with TAR32 for Win32]),
			    [use_tar32=yes])
if test x"${use_tar32}" = "xyes"; then
AC_DEFINE(USE_TAR32,[], [Make with TAR32 for Win32])
TAR32_LIBS="-L. -ltar32"
else
TAR32_LIBS=""
fi
fi
AC_SUBST(TAR32_LIBS)

dnl AM_PATH_GLIB_2_0(2.32.0,, AC_MSG_ERROR([glib-2.x is required]), gthread gobject)
PKG_CHECK_MODULES(GLIB, glib-2.0 >= 2.32, dummy=yes,
			AC_MSG_ERROR(GLib >= 2.32 is required))
GLIB_CFLAGS="$GLIB_CFLAGS"
GLIB_LIBS="$GLIB_LIBS"
AC_SUBST(GLIB_CFLAGS)
AC_SUBST(GLIB_LIBS)

dnl GTK 3 or 2 
AC_ARG_WITH(gtk2,
             AC_HELP_STRING([--with-gtk2],
                        [use Gtk+2 instead of Gtk+3]),
                        [use_gtk2=yes])
if test x"${use_gtk2}" = "xyes"; then
dnl Gtk+2
PKG_CHECK_MODULES(GTK2, gtk+-2.0 >= 2.24, dummy=yes,
			AC_MSG_ERROR(Gtk+2 >= 2.24 is required))
GTK_CFLAGS="$GTK2_CFLAGS"
GTK_LIBS="$GTK2_LIBS"
AC_DEFINE([USE_GTK2],[],[Use Gtk+2.x instead of Gtk+3])
else
dnl GTK+3
if test x"${use_osx}" = "xyes"; then
PKG_CONFIG_PATH="${PKG_CONFIG_PATH}:/usr/local/opt/libffi/lib/pkgconfig"
fi
PKG_CHECK_MODULES(GTK3, gtk+-3.0 >= 3.22, dummy=yes,
			AC_MSG_ERROR(Gtk+3 >= 3.22 is required))
GTK_CFLAGS="$GTK3_CFLAGS"
GTK_LIBS="$GTK3_LIBS"
AC_DEFINE([USE_GTK3],[],[Use Gtk+3.x instead of Gtk+2])
fi
AC_SUBST(GTK_LIBS)
AC_SUBST(GTK_CFLAGS)

dnl Checks for iconv
AM_ICONV

dnl gresource
AC_PATH_PROG([GLIB_COMPILE_RESOURCES], [glib-compile-resources])

dnl for gettext
ALL_LINGUAS="ja uk"
AM_GNU_GETTEXT
GETTEXT_PACKAGE=macopix
AC_SUBST(LOCALEDIR)
AC_SUBST(GETTEXT_PACKAGE)
AC_DEFINE_UNQUOTED(GETTEXT_PACKAGE,"$GETTEXT_PACKAGE", [Define text domain.])
AM_GLIB_GNU_GETTEXT
POMAKEFILEDEPS="POTFILES.in"
AC_SUBST(POMAKEFILEDEPS)
dnl AM_GNU_GETTEXT
dnl AC_CHECK_FUNC(gettext, AC_CHECK_LIB(intl, gettext))
if test x"${use_win32}" = "xyes"; then
localedir='share/locale'
WIN32_ICONV_LIBS="-liconv"
dnl  a temoral measure to solve a problem on MinGW glib iconv
else
if test x"${use_osx}" = "xyes"; then
PKG_CONFIG_PATH="${PKG_CONFIG_PATH}:/usr/local/opt/libffi/lib/pkgconfig"
localedir='share/locale'
WIN32_ICONV_LIBS="-liconv"
else
WIN32_ICONV_LIBS=""
localedir='${prefix}/${DATADIRNAME}/locale'
fi
fi
AC_SUBST(WIN32_ICONV_LIBS)

AC_ARG_WITH(localedir,
	[  --with-localedir=DIR    Locale directory],
	[localedir="$withval"])
AC_SUBST(localedir)

themedir='${datadir}/${PACKAGE}/icons'
AC_ARG_WITH(themedir,
	[  --with-themedir=DIR     Icon theme directory],
	[themedir="$withval"])
AC_SUBST(themedir)

dnl for Maintainer mode (DISABLE_DEPRECATED)
AC_ARG_WITH(maintain,
		 AC_HELP_STRING([--with-maintain],
		            [For Maintainer]),
			    [use_maint=yes])
             

if test x"${use_maint}" = "xyes"; then
        DISABLE_DEPRECATED="-g -DG_DISABLE_DEPRECATED -DGDK_DISABLE_DEPRECATED -DGDK_PIXBUF_DISABLE_DEPRECATED -DGTK_DISABLE_DEPRECATED"
else
        DISABLE_DEPRECATED=""
fi

dnl GtkMACIntegration
 AC_ARG_WITH(gtkmac,
 		 AC_HELP_STRING([--with-gtkmac],
		            [make with gtkmacintegration]),
			    [with_gtkmac=yes])
 
if test x"${with_gtkmac}" = "xyes"; then
  if test "$os_osx" == yes; then
    PKG_CHECK_MODULES(GTKMAC, gtk-mac-integration-gtk3 >= 2.1, dummy=yes,
			AC_MSG_ERROR(gtk-mac-integration >= 2.1 is required))
    GTKMAC_CFLAGS="$GTKMAC_CFLAGS"
    GTKMAC_LIBS="$GTKMAC_LIBS"
    AC_DEFINE([USE_GTKMACINTEGRATION],[],[With gtkmacintegration])
  else
    GTKMAC_CFALGS=""
    GTKMAC_LIBS=""
  fi
else
  GTKMAC_CFALGS=""
  GTKMAC_LIBS=""
fi
AC_SUBST(GTKMAC_LIBS)
AC_SUBST(GTKMAC_CFLAGS)


AC_SUBST(DISABLE_DEPRECATED)


dnl AC_PROG_INTLTOOL


dnl #######################################################################
dnl # Cairo (Translucent) support
dnl #######################################################################

AC_ARG_ENABLE(cairo,
	[  --disable-cairo	Disable translucent drawing with Cairo],
	[ac_cv_enable_cairo=$enableval], [ac_cv_enable_ssl=yes])

if test $ac_cv_enable_cairo = no; then
   			AC_DEFINE(DISABLE_CAIRO, 1, Define if you wanna disable Cairo.)
fi

dnl #######################################################################
dnl # SSL support
dnl #######################################################################

AC_ARG_ENABLE(ssl,
	[  --disable-ssl           Disable SSL support using OpenSSL/GNUTLS],
	[ac_cv_enable_ssl=$enableval], [ac_cv_enable_ssl=yes])

dnl Check for GNUTLS
AC_MSG_CHECKING([whether to use SSL])
if test $ac_cv_enable_ssl = yes; then
   	AC_MSG_RESULT(yes)

AC_ARG_WITH(gnutls,
	 AC_HELP_STRING([--with-gnutls],
		            [Make with GNUTLS instead of OpenSSL]),
			    [use_gnutls=yes])
dnl #
dnl # Check for GnuTLS if it's specified.
dnl #
AC_MSG_CHECKING([whether to use GNUTLS])
if test "x$use_gnutls" = "xyes"; then
	AC_MSG_RESULT(yes)
	use_gnutls="no"
	prefix=`eval echo $prefix`

	AC_ARG_WITH(gnutls-includes,
		[  --with-gnutls-includes=PREFIX   location of GnuTLS includes.],
		[ with_gnutls_includes="$withval" ],
		[ with_gnutls_includes="$prefix/include" ])

	have_gnutls_includes="no"

	if test "x$with_gnutls_includes" != "xno"; then
		CPPFLAGS_save="$CPPFLAGS"

		AC_MSG_CHECKING(for GnuTLS includes)
		AC_MSG_RESULT("")

		CPPFLAGS="$CPPFLAGS -I$with_gnutls_includes"
		AC_CHECK_HEADERS(gnutls/gnutls.h, [ gnutls_includes="yes" ])
		CPPFLAGS="$CPPFLAGS_save"

		if test "x$gnutls_includes" != "xno" -a \
		        "x$gnutls_includes" != "x"; then
			have_gnutls_includes="yes"

			if test "x$with_gnutls_includes" != "xNONE/include"; then
				GNUTLS_CFLAGS="-I$with_gnutls_includes"
			fi
		else
			GNUTLS_CFLAGS=""
		fi
	else
		AC_MSG_CHECKING(for GnuTLS includes)
		AC_MSG_RESULT(no)
	fi

	AC_ARG_WITH(gnutls-libs,
		[AC_HELP_STRING([--with-gnutls-libs=PREFIX], [location of GnuTLS libraries.])],
		[ with_gnutls_libs="$withval" ])

	if test "x$with_gnutls_libs"     != "xno" -a \
	        "x$have_gnutls_includes" != "xno"; then

		LIBS_save="$LIBS"

		case $with_gnutls_libs in
			""|-L*) ;;
			*) with_gnutls_libs="-L$with_gnutls_libs" ;;
		esac

		AC_CACHE_CHECK([for GnuTLS libraries], gnutls_libs,
		[
			LIBS="$LIBS $with_gnutls_libs -lgnutls -lgcrypt -lgnutls-openssl"
			AC_TRY_LINK_FUNC(gnutls_init, gnutls_libs="yes", gnutls_libs="no")
			LIBS="$LIBS_save"
		])

		if test "x$gnutls_libs" != "xno"; then
			AC_DEFINE(USE_SSL, 1, Define if you use SSL.)
			AC_DEFINE(USE_GNUTLS, 1, Define if you use GNUTLS to support SSL.)
			msg_gnutls="GnuTLS"
			GNUTLS_LIBS="$with_gnutls_libs -lgnutls -lgcrypt -lgnutls-openssl"

			use_gnutls="yes"
		else
			GNUTLS_CFLAGS=""
			GNUTLS_LIBS=""
		fi
	else
		AC_MSG_CHECKING(for GnuTLS libraries)
		AC_MSG_RESULT(no)
	fi
else
	AC_MSG_RESULT(no)
	GNUTLS_CFLAGS=""
	GNUTLS_LIBS=""
fi

AC_SUBST(GNUTLS_CFLAGS)
AC_SUBST(GNUTLS_LIBS)


dnl Check for OpenSSL
AC_MSG_CHECKING([whether to use OpenSSL])
if test "x$use_gnutls" != "xyes"; then
	AC_MSG_RESULT(yes)
	dnl First check with pkg-config
	PKG_CHECK_MODULES(OPENSSL, openssl, [:], [:])
	if test "x$OPENSSL_LIBS" != "x"; then
		CFLAGS="$CFLAGS $OPENSSL_CFLAGS"
		LIBS="$LIBS $OPENSSL_LIBS"
		AC_MSG_CHECKING([if openssl is available])
		AC_MSG_RESULT(yes)
		AC_DEFINE(USE_SSL, 1, Define if you use SSL.)
		AC_DEFINE(USE_OPENSSL, 1, Define if you use OPENSSL to support SSL.)
	else
		dnl If pkg-config fails, run compile/link test.
		AC_MSG_CHECKING([if openssl is available])
		ac_save_LIBS="$LIBS"
		if test x"${use_win32}" = "xyes"; then
			LIBS="$LIBS -lssl32 -leay32"
		else
			LIBS="$LIBS -lssl -lcrypto"
		fi
		AC_TRY_LINK([
#include <openssl/opensslv.h>
],		[ return OPENSSL_VERSION_NUMBER; ],
		[ AC_MSG_RESULT(yes)
		  AC_DEFINE(USE_SSL, 1, Define if you use SSL.) ],
		[ AC_MSG_RESULT(no)
		  LIBS="$ac_save_LIBS"
		  ac_cv_enable_ssl=no ])
		AC_DEFINE(USE_OPENSSL, 1, Define if you use OPENSSLto support SSL.)
	fi
else
	AC_MSG_RESULT(no)
fi
	AC_MSG_RESULT(no)
fi


manualdir='${prefix}/${DATADIRNAME}/${PACKAGE}/manual'
AC_SUBST(manualdir)

prefixdir='${prefix}/share/'
AC_SUBST(prefixdir)
if test x"${use_win32}" = "xyes"; then
commondir='share/${PACKAGE}/'
else
commondir='${prefix}/share/${PACKAGE}/'
fi
AC_SUBST(commondir)

AC_OUTPUT([
Makefile
po/Makefile.in
intl/Makefile
m4/Makefile
src/version.h
src/macopix_res.rc
src/Makefile
src/icons/Makefile
])




dnl Output the configuration summary
echo ""
echo "##############################################################################"
if test x"${use_win32}" = "xyes"; then
echo " $PACKAGE $VERSION build for Win32 "
fi
if test x"${use_gtk2}" = "xyes"; then
echo " $PACKAGE $VERSION  with Gtk+2 "
else
echo " $PACKAGE $VERSION  with Gtk+3 "
fi
echo ""
if test $ac_cv_enable_ssl = yes; then
if test "x$use_gnutls" != "xyes"; then
echo "  Compiling with OpenSSL to support SSL"
echo "  (If you want to use GNUTLS instead of OpenSSL,"
echo "                                     use \"./configure --with-gnutls\" .)"
else
echo "  Compiling with GNUTLS to support SSL"
fi
else
echo "  Compiling without SSL support"
fi
echo "##############################################################################"
echo ""
echo "The binary will be installed in $prefix/bin"
echo "The common directory will be $prefix/share/$PACKAGE/"
echo ""
echo "Configure finished, type 'make' to build."
