AC_PREREQ([2.69])

AC_INIT([authinfo], [0.1], [https://github.com/aartamonau/authinfo/issues])
AM_INIT_AUTOMAKE([subdir-objects foreign])
LT_INIT([static])

AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_SRCDIR([src/cli.c])
AC_CONFIG_FILES([Makefile docs/Makefile docs/Doxyfile authinfo.pc]
                [dist/archlinux/PKGBUILD])
AC_CONFIG_HEADERS([config.h])

AC_PROG_CC_C99

if test "x${ac_cv_prog_cc_c99}" = xno; then
  AC_MSG_ERROR([A compiler supporting c99 is required])
fi

AC_CHECK_DECL([__GNUC__], [gcc=yes], [gcc=no])
AM_CONDITIONAL([GCC], [test x$gcc = xyes])

AC_DEFINE_DIR([SYSCONF_DIR], [sysconfdir],
  [System configuration directory (typically /etc).])

AM_PROG_CC_C_O

PKG_CHECK_MODULES([CHECK], [check >= 0.9.0], [have_check=yes], [have_check=no])
AM_CONDITIONAL([HAVE_CHECK], [test x$have_check = xyes])

AC_ARG_ENABLE([debug],
  [AS_HELP_STRING([--enable-debug],
          [enable debug build @<:@default=no@:>@])],
  [enable_debug="$enableval"],
  [enable_debug="no"])
AM_CONDITIONAL([DEBUG], [test x$enable_debug = xyes])

AC_ARG_ENABLE([sanitizer],
  [AS_HELP_STRING([--enable-sanitizer],
          [enable address and undefined behavior sanitizers @<:@default=no@:>@])],
  [enable_sanitizer="$enableval"],
  [enable_sanitizer="no"])
AM_CONDITIONAL([SANITIZER], [test x$enable_sanitizer = xyes])

AC_CHECK_PROG([have_lcov], [lcov], [yes])
AM_CONDITIONAL([HAVE_LCOV], [test x$have_lcov = xyes])

AC_CHECK_PROG([have_xdg_open], [xdg-open], [yes])
AM_CONDITIONAL([HAVE_XDG_OPEN], [test x$have_xdg_open = xyes])

AC_CHECK_PROG([have_doxygen], [doxygen], [yes])
AM_CONDITIONAL([HAVE_DOXYGEN], [test x$have_doxygen = xyes])

if test x$have_doxygen != xyes; then
   AC_MSG_WARN(
     [Doxygen not found. API documentation won't be generated])
fi

dnl TODO: refine version restriction
AM_PATH_GPGME_PTHREAD([1.4.0], [have_gpgme=yes], [have_gpgme=no])

if test x$have_gpgme = xno; then
   AC_MSG_ERROR([Couldn't find GPGME.])
fi

dnl TODO: version
AM_PATH_LIBASSUAN([2.1.0], [have_libassuan=yes], [have_libassuan=no])

if test x$have_libassuan = xno; then
   AC_MSG_ERROR([Couldn't find libassuan.])
fi

AC_ARG_ENABLE([cli],
  [AS_HELP_STRING([--enable-cli],
          [build cli tool @<:@default=yes@:>@])],
   [enable_cli="$enableval"],
   [enable_cli="yes"])

AM_CONDITIONAL([BUILD_CLI], [test x$enable_cli = xyes])

if test x$enable_cli = xyes; then
  AC_CHECK_FUNCS([getopt_long])
fi

AC_ARG_ENABLE([python],
  [AS_HELP_STRING([--enable-python],
          [install python bindings @<:@default=yes@:>@])],
   [enable_python="$enableval"],
   [enable_python="yes"])

if test x$enable_python = xyes; then
   if test x$enable_shared != xyes; then
      AC_MSG_ERROR("--enable-python requires --enable-shared")
   fi

   AM_PATH_PYTHON([2.5])
   AC_CONFIG_FILES([python/__init__.py])
fi

AM_CONDITIONAL([ENABLE_PYTHON], [test x$enable_python = xyes])

LIBAUTHINFO_INTERFACE=0
LIBAUTHINFO_REVISION=0
LIBAUTHINFO_AGE=0
LIBAUTHINFO_VERSION_INFO=${LIBAUTHINFO_INTERFACE}:${LIBAUTHINFO_REVISION}:${LIBAUTHINFO_AGE}

AC_SUBST([LIBAUTHINFO_INTERFACE])
AC_SUBST([LIBAUTHINFO_REVISION])
AC_SUBST([LIBAUTHINFO_AGE])
AC_SUBST([LIBAUTHINFO_VERSION_INFO])

AC_OUTPUT
