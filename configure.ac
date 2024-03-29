# -*- Autoconf -*-

# Process this file with autoconf to produce a configure script.

AC_PREREQ(2.59)
AC_INIT([v4l-utils],[1.17.0])

AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_SRCDIR([lib/libv4l2/libv4l2.c])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_FILES([Makefile
	lib/Makefile
	lib/libv4lconvert/Makefile
	lib/libv4l2/Makefile
	lib/libv4l1/Makefile
	lib/libdvbv5/Makefile
	lib/libv4l2rds/Makefile
	lib/libv4l-mplane/Makefile

	utils/Makefile
	utils/libv4l2util/Makefile
	utils/libmedia_dev/Makefile
	utils/dvb/Makefile
	utils/keytable/Makefile
	utils/keytable/bpf_protocols/Makefile
	utils/ir-ctl/Makefile
	utils/cx18-ctl/Makefile
	utils/ivtv-ctl/Makefile
	utils/media-ctl/Makefile
	utils/v4l2-compliance/Makefile
	utils/v4l2-ctl/Makefile
	utils/v4l2-dbg/Makefile
	utils/v4l2-sysfs-path/Makefile
	utils/qv4l2/Makefile
	utils/cec-ctl/Makefile
	utils/cec-ctl/cec-ctl.1
	utils/cec-compliance/Makefile
	utils/cec-compliance/cec-compliance.1
	utils/cec-follower/Makefile
	utils/cec-follower/cec-follower.1
	utils/qvidcap/Makefile
	utils/rds-ctl/Makefile

	contrib/Makefile
	contrib/freebsd/Makefile
	contrib/test/Makefile
	contrib/gconv/Makefile
	contrib/cobalt-ctl/Makefile
	contrib/decode_tm6000/Makefile
	contrib/xc3028-firmware/Makefile
	contrib/rds-saa6588/Makefile

	v4l-utils-po/Makefile.in
	libdvbv5-po/Makefile.in

	v4l-utils.spec
	lib/libv4lconvert/libv4lconvert.pc
	lib/libv4l1/libv4l1.pc
	lib/libv4l2/libv4l2.pc
	lib/libdvbv5/libdvbv5.pc
	lib/libv4l2rds/libv4l2rds.pc
	utils/media-ctl/libmediactl.pc
	utils/media-ctl/libv4l2subdev.pc

	lib/include/libdvbv5/libdvb-version.h

	utils/qv4l2/qv4l2.1
	utils/qvidcap/qvidcap.1
	utils/v4l2-compliance/v4l2-compliance.1
	utils/v4l2-ctl/v4l2-ctl.1
	utils/keytable/ir-keytable.1
	utils/keytable/rc_keymap.5
	utils/ir-ctl/ir-ctl.1
	utils/dvb/dvb-fe-tool.1
	utils/dvb/dvbv5-scan.1
	utils/dvb/dvb-format-convert.1
	utils/dvb/dvbv5-zap.1
])

AC_GNU_SOURCE

AM_INIT_AUTOMAKE([1.9 subdir-objects no-dist-gzip dist-bzip2 -Wno-portability]) # 1.10 is needed for target_LIBTOOLFLAGS

AM_MAINTAINER_MODE
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

# Checks for programs.
AC_PROG_CXX
AC_PROG_CC_C99
AC_PROG_INSTALL
AC_PROG_LIBTOOL
AC_PROG_MKDIR_P
AC_PROG_LN_S
PKG_PROG_PKG_CONFIG
DX_DOT_FEATURE(ON)
DX_INIT_DOXYGEN($PACKAGE_NAME, doxygen_libdvbv5.cfg)
ALL_LINGUAS=""
AM_GNU_GETTEXT_VERSION([0.19.8])
AM_GNU_GETTEXT([external])

LIBDVBV5_DOMAIN="libdvbv5"
AC_DEFINE([LIBDVBV5_DOMAIN], "libdvbv5", [libdvbv5 domain])
AC_SUBST(LIBDVBV5_DOMAIN)

# Define localedir
AC_DEFUN([V4L_EXPAND_PREFIX], [
	$1=$2
	dnl expanding twice, since from autoconf 2.60 on, $datadir refers to
	dnl $datarootdir which in turn refers to $prefix
	$1=`(	test "x$prefix" = xNONE && prefix="$ac_default_prefix"
		eval tmp_v4l_prefix=\""[$]$1"\"
		eval echo $tmp_v4l_prefix
		)`
])

V4L_EXPAND_PREFIX(v4l_localedir, "$datadir/locale")
localedir="\$(datadir)/locale"

AC_ARG_WITH(localedir,
	[  --with-localedir=PATH      Where the locale files are installed ])
if test "x$with_localedir" != "x"; then
	v4l_localedir="$with_localedir"
	localedir="$with_localedir"
fi

AC_DEFINE_UNQUOTED(LOCALEDIR, "$v4l_localedir", [locale directory])
AC_SUBST(localedir)

# sync-with-kernel and keytable install could make use of them
#AC_PROG_GREP
#AC_PROG_SED
gl_PROMOTED_TYPE_MODE_T
gl_VISIBILITY

AC_CHECK_HEADERS([sys/klog.h])
AC_CHECK_FUNCS([klogctl])

AC_CACHE_CHECK([for ioctl with POSIX signature],
  [gl_cv_func_ioctl_posix_signature],
  [AC_COMPILE_IFELSE(
     [AC_LANG_PROGRAM(
        [[#include <sys/ioctl.h>]],
        [[int ioctl (int, int, ...);]])
     ],
     [gl_cv_func_ioctl_posix_signature=yes],
     [gl_cv_func_ioctl_posix_signature=no])
  ])
if test "x$gl_cv_func_ioctl_posix_signature" = xyes; then
  AC_DEFINE([HAVE_POSIX_IOCTL], [1], [Have ioctl with POSIX signature])
fi

AC_CHECK_FUNCS([__secure_getenv secure_getenv])
AC_HEADER_MAJOR

# Check host os
case "$host_os" in
  *linux*)
    linux_os="yes"
    ;;
  *freebsd*)
    freebsd_os="yes"
    ;;
esac

AM_CONDITIONAL([LINUX_OS], [test x$linux_os = xyes])

# Checks for dependencies

PKG_CHECK_MODULES(X11, [x11], [x11_pkgconfig=yes], [x11_pkgconfig=no])
AC_SUBST([X11_CFLAGS])
AC_SUBST([X11_LIBS])
AM_CONDITIONAL([HAVE_X11], [test x$x11_pkgconfig = xyes])

PKG_CHECK_MODULES([LIBELF], [libelf], [libelf_pkgconfig=yes], [libelf_pkgconfig=no])
AC_SUBST([LIBELF_CFLAGS])
AC_SUBST([LIBELF_LIBS])
if test "x$libelf_pkgconfig" = "xyes"; then
  AC_CHECK_PROG([CLANG], clang, clang)
else
  AC_MSG_WARN(libelf library not available)
fi

AS_IF([test "x$x11_pkgconfig" = xyes],
      [PKG_CHECK_MODULES(GL, [gl], [gl_pkgconfig=yes], [gl_pkgconfig=no])], [gl_pkgconfig=no])
AC_SUBST([GL_CFLAGS])
AC_SUBST([GL_LIBS])
AM_CONDITIONAL([HAVE_GL], [test x$gl_pkgconfig = xyes])

AS_IF([test "x$gl_pkgconfig" = xyes],
      [PKG_CHECK_MODULES(GLU, [glu], [glu_pkgconfig=yes], [glu_pkgconfig=no])], [glu_pkgconfig=no])

AC_SUBST([GLU_CFLAGS])
AC_SUBST([GLU_LIBS])
AM_CONDITIONAL([HAVE_GLU], [test x$glu_pkgconfig = xyes])

AC_ARG_WITH([jpeg],
            AS_HELP_STRING([--without-jpeg], [Do not use jpeg library]),
            [],
            [with_jpeg=yes])

AS_IF([test "x$with_jpeg" != xno],
      [AC_CHECK_HEADER([jpeglib.h],
                       [AC_CHECK_LIB([jpeg],
                                     [jpeg_read_header],
                                     [have_jpeg=yes
                                      JPEG_LIBS="-ljpeg"
                                      AC_DEFINE([HAVE_JPEG],[1],[whether we use libjpeg])],
                                     [have_jpeg=no
                                      AC_MSG_WARN(cannot find libjpeg (v6 or later required))])],
                       [have_jpeg=no
                        AC_MSG_WARN(cannot find libjpeg)])])

AM_CONDITIONAL([HAVE_JPEG], [test x$have_jpeg = xyes])

QT_VERSION="none"

PKG_CHECK_MODULES(QT5, [Qt5Core >= 5.0 Qt5Gui >= 5.0 Qt5Widgets >= 5.0], [qt_pkgconfig=true], [qt_pkgconfig=false])
PKG_CHECK_MODULES(QT5, [Qt5Core >= 5.4 Qt5Gui >= 5.4 Qt5Widgets >= 5.4], [qt54_pkgconfig=true], [qt54_pkgconfig=false])
if test "x$qt_pkgconfig" = "xtrue"; then
   QT_CFLAGS="$QT5_CFLAGS -fPIC"
   QT_LIBS="$QT5_LIBS"
   AC_SUBST(QT_CFLAGS)
   AC_SUBST(QT_LIBS)
   AC_CHECK_PROGS(MOC, [moc-qt5 moc])
   AC_CHECK_PROGS(UIC, [uic-qt5 uic])
   AC_CHECK_PROGS(RCC, [rcc-qt5 rcc])
   AC_SUBST(MOC)
   AC_SUBST(UIC)
   AC_SUBST(RCC)
   if test "x$qt54_pkgconfig" = "xtrue"; then
      PKG_CHECK_MODULES(QT5GL, [Qt5OpenGL >= 5.4 gl], [qt_pkgconfig_gl=true], [qt_pkgconfig_gl=false])
   else
      PKG_CHECK_MODULES(QT5GL, [Qt5OpenGL >= 5.0 gl], [qt_pkgconfig_gl=true], [qt_pkgconfig_gl=false])
   fi
   if test "x$qt_pkgconfig_gl" = "xtrue"; then
      SAVED_CXXFLAGS="${CXXFLAGS}"
      CXXFLAGS="${CXXFLAGS} ${QT_CFLAGS} ${QT5GL_CFLAGS}"
      AC_LANG_PUSH([C++])
      AC_MSG_CHECKING([for Qt5 Desktop OpenGL])
      AC_COMPILE_IFELSE([AC_LANG_SOURCE(
[[
#define GL_GLEXT_PROTOTYPES
#define QT_NO_OPENGL_ES_2

#include <QGLWidget>
#include <QGLShader>
#include <QGLShaderProgram>
#include <QGLFunctions>

#ifndef GL_UNSIGNED_INT_8_8_8_8
#error Missing OpenGL Extensions
#endif
]])],
         [qt_desktop_opengl=yes],
         [qt_desktop_opengl=no])
      AC_MSG_RESULT([$qt_desktop_opengl])
      CXXFLAGS="${SAVED_CXXFLAGS}"
      AC_LANG_POP
   fi

   if test "x$qt_desktop_opengl" = "xyes"; then
      QTGL_CFLAGS="$QT5GL_CFLAGS -fPIC"
      QTGL_LIBS="$QT5GL_LIBS"
      AC_SUBST(QTGL_CFLAGS)
      AC_SUBST(QTGL_LIBS)
      AC_DEFINE([HAVE_QTGL], [1], [qt has opengl support])
      if test "x$qt54_pkgconfig" = "xtrue"; then
        QT_VERSION="v5.4 with QtGL"
      else
        QT_VERSION="v5 with QtGL"
      fi
   else
      if test "x$qt54_pkgconfig" = "xtrue"; then
        QT_VERSION="v5.4"
      else
        QT_VERSION="v5"
      fi
      AC_MSG_WARN(Qt5 Desktop OpenGL is not available)
   fi
else
   AC_MSG_WARN(Qt5 or higher is not available)
fi


PKG_CHECK_MODULES(ALSA, [alsa], [alsa_pkgconfig=true], [alsa_pkgconfig=false])
if test "x$alsa_pkgconfig" = "xtrue"; then
   AC_DEFINE([HAVE_ALSA], [1], [alsa library is present])
else
   AC_MSG_WARN(ALSA library not available)
fi

PKG_CHECK_MODULES(libudev, libudev, have_libudev=yes, have_libudev=no)
if test "x$have_libudev" = "xyes"; then
	AC_DEFINE([HAVE_LIBUDEV], [], [Use libudev])
	LIBUDEV_CFLAGS="$libudev_CFLAGS"
	LIBUDEV_LIBS="$libudev_LIBS"
	AC_SUBST(LIBUDEV_CFLAGS)
	AC_SUBST(LIBUDEV_LIBS)
else
   AC_MSG_WARN(udev library not available)
fi

AC_SUBST([JPEG_LIBS])

# Check for pthread

AS_IF([test x$enable_shared != xno],
	[AX_PTHREAD([have_pthread=yes
		    AC_DEFINE([HAVE_PTHREAD],[1],[Define if you have POSIX threads libraries and header files.])],
		   [have_pthread=no])],
	[have_pthread=no])

# The dlopen() function is in the C library for *BSD and in
# libdl on GLIBC-based systems

dl_saved_libs=$LIBS
  AC_SEARCH_LIBS([dlopen],
                 [dl],
                 [test "$ac_cv_search_dlopen" = "none required" || DLOPEN_LIBS=$ac_cv_search_dlopen],
                 [AC_MSG_WARN([unable to find the dlopen() function])])
  AC_SUBST([DLOPEN_LIBS])
LIBS=$dl_saved_libs

AC_CHECK_HEADER([argp.h],,AC_MSG_ERROR(Cannot continue: argp.h not found))
argp_saved_libs=$LIBS
  AC_SEARCH_LIBS([argp_parse],
                 [argp],
                 [test "$ac_cv_search_argp_parse" = "none required" || ARGP_LIBS=$ac_cv_search_argp_parse],
                 [AC_MSG_ERROR([unable to find the argp_parse() function])])
  AC_SUBST([ARGP_LIBS])
LIBS=$argp_saved_libs

AC_CHECK_FUNCS([fork], AC_DEFINE([HAVE_LIBV4LCONVERT_HELPERS],[1],[whether to use libv4lconvert helpers]))
AM_CONDITIONAL([HAVE_LIBV4LCONVERT_HELPERS], [test x$ac_cv_func_fork = xyes])

AC_CHECK_HEADER([linux/i2c-dev.h], [linux_i2c_dev=yes], [linux_i2c_dev=no])
AM_CONDITIONAL([HAVE_LINUX_I2C_DEV], [test x$linux_i2c_dev = xyes])

AM_ICONV

# directories

AC_ARG_WITH(libv4l1subdir,
	AS_HELP_STRING([--with-libv4l1subdir=DIR], [set libv4l1 library subdir (default=libv4l)]),
	[],
	[with_libv4l1subdir="libv4l"])

AC_ARG_WITH(libv4l2subdir,
	AS_HELP_STRING([--with-libv4l2subdir=DIR], [set libv4l2 library subdir (default=libv4l)]),
   	[],
	[with_libv4l2subdir="libv4l"])

AC_ARG_WITH(libv4lconvertsubdir,
	AS_HELP_STRING([--with-libv4lconvertsubdir=DIR], [set libv4lconvert library subdir (default=libv4l)]),
	[],
	[with_libv4lconvertsubdir="libv4l"])

AC_ARG_WITH(udevdir,
	AS_HELP_STRING([--with-udevdir=DIR], [set udev directory]),
   	[],
	[with_udevdir=`$PKG_CONFIG --variable=udevdir udev || echo /lib/udev`])

# Generic check: works with most distributions
def_gconv_dir=`for i in /lib64 /usr/lib64 /usr/local/lib64 /lib /usr/lib /usr/local/lib; do if @<:@ -d \$i/gconv @:>@; then echo \$i/gconv; break; fi; done`

# Debian/Ubuntu-specific check: should be aligned with the debian package
AS_IF([test "x${def_gconv_dir}" = "x" ],
      def_gconv_dir=`for i in mips64el-linux-gnuabi64 mips64el-linux-gnuabin32 mips64-linux-gnuabi64 mips64-linux-gnuabin32 mipsisa64r6el-linux-gnuabi64 mipsisa64r6el-linux-gnuabin32 mipsisa64r6-linux-gnuabi64 mipsisa64r6-linux-gnuabin32 powerpc64-linux-gnu sparc64-linux-gnu x86_64-linux-gnu x86_64-linux-gnux32 alphaev67-linux-gnu arm-linux-gnueabi arm-linux-gnueabihf i686-kfreebsd-gnu i686-linux-gnu mipsel-linux-gnu mipsisa32r6el-linux-gnu mipsisa32r6-linux-gnu mips-linux-gnu powerpc-linux-gnu s390-linux-gnu sparcv9-linux-gnu; do if @<:@ -d /usr/lib/\$i/gconv @:>@; then echo /usr/lib/\$i/gconv; break; fi; done`)

AC_ARG_WITH(gconvdir,
	AS_HELP_STRING([--with-gconvdir=DIR],
		       [set system's gconv directory (default is to autodetect)]),
	[],
	[with_gconvdir=$def_gconv_dir])

AC_SUBST([libv4l1subdir], [$with_libv4l1subdir])
AC_SUBST([libv4l2subdir], [$with_libv4l2subdir])
AC_SUBST([libv4l1privdir], [$libdir/$with_libv4l1subdir])
AC_SUBST([libv4l2privdir], [$libdir/$with_libv4l2subdir])
AC_SUBST([libv4l2plugindir], [$libdir/$with_libv4l2subdir/plugins])
AC_SUBST([libv4lconvertprivdir], [$libdir/$with_libv4lconvertsubdir])
AC_SUBST([keytablesystemdir], [$with_udevdir/rc_keymaps])
AC_SUBST([keytableuserdir], [$sysconfdir/rc_keymaps])
AC_SUBST([udevrulesdir], [$with_udevdir/rules.d])
AC_SUBST([pkgconfigdir], [$libdir/pkgconfig])
AC_SUBST([gconvsysdir], [$with_gconvdir])

AC_DEFINE_UNQUOTED([V4L_UTILS_VERSION], ["$PACKAGE_VERSION"], [v4l-utils version string])
AC_DEFINE_DIR([LIBV4L1_PRIV_DIR], [libv4l1privdir], [libv4l1 private lib directory])
AC_DEFINE_DIR([LIBV4L2_PRIV_DIR], [libv4l2privdir], [libv4l2 private lib directory])
AC_DEFINE_DIR([LIBV4L2_PLUGIN_DIR], [libv4l2plugindir], [libv4l2 plugin directory])
AC_DEFINE_DIR([LIBV4LCONVERT_PRIV_DIR], [libv4lconvertprivdir], [libv4lconvert private lib directory])
AC_DEFINE_DIR([IR_KEYTABLE_SYSTEM_DIR], [keytablesystemdir], [ir-keytable preinstalled tables directory])
AC_DEFINE_DIR([IR_KEYTABLE_USER_DIR], [keytableuserdir], [ir-keytable user defined tables directory])

MAJOR=`echo "$PACKAGE_VERSION" | perl -ne 'print $1 if (m/^(\d+)\.(\d+)\.(\d+)/)'`
MINOR=`echo "$PACKAGE_VERSION" | perl -ne 'print $2 if (m/^(\d+)\.(\d+)\.(\d+)/)'`
PATCH=`echo "$PACKAGE_VERSION" | perl -ne 'print $3 if (m/^(\d+)\.(\d+)\.(\d+)/)'`

AC_SUBST(MAJOR)
AC_SUBST(MINOR)
AC_SUBST(PATCH)
AC_SUBST(V4L_UTILS_VERSION)

# options

AC_ARG_ENABLE(libdvbv5,
  AS_HELP_STRING([--disable-libdvbv5], [disable libdvbv5 compilation]),
  [case "${enableval}" in
     yes | no ) ;;
     *) AC_MSG_ERROR(bad value ${enableval} for --disable-libdvbv5) ;;
   esac]
)

AC_ARG_ENABLE(dyn-libv4l,
  AS_HELP_STRING([--disable-dyn-libv4l], [disable dynamic libv4l support]),
  [case "${enableval}" in
     yes | no ) ;;
     *) AC_MSG_ERROR(bad value ${enableval} for --disable-dyn-libv4l) ;;
   esac]
)

AC_ARG_ENABLE(v4l-utils,
  AS_HELP_STRING([--disable-v4l-utils], [disable v4l-utils compilation]),
  [case "${enableval}" in
     yes | no ) ;;
     *) AC_MSG_ERROR(bad value ${enableval} for --disable-v4l-utils) ;;
   esac]
)

AC_ARG_ENABLE(v4l2-compliance-libv4l,
  AS_HELP_STRING([--disable-v4l2-compliance-libv4l], [disable use of libv4l in v4l2-compliance]),
  [case "${enableval}" in
     yes | no ) ;;
     *) AC_MSG_ERROR(bad value ${enableval} for --disable-v4l2-compliance-libv4l) ;;
   esac]
)

AC_ARG_ENABLE(v4l2-ctl-libv4l,
  AS_HELP_STRING([--disable-v4l2-ctl-libv4l], [disable use of libv4l in v4l2-ctl]),
  [case "${enableval}" in
     yes | no ) ;;
     *) AC_MSG_ERROR(bad value ${enableval} for --disable-v4l2-ctl-libv4l) ;;
   esac]
)

AC_ARG_ENABLE(v4l2-ctl-stream-to,
  AS_HELP_STRING([--disable-v4l2-ctl-stream-to], [disable use of --stream-to in v4l2-ctl]),
  [case "${enableval}" in
     yes | no ) ;;
     *) AC_MSG_ERROR(bad value ${enableval} for --disable-v4l2-ctl-stream-to) ;;
   esac]
)

AC_ARG_ENABLE(qv4l2,
  AS_HELP_STRING([--disable-qv4l2], [disable qv4l2 compilation]),
  [case "${enableval}" in
     yes | no ) ;;
     *) AC_MSG_ERROR(bad value ${enableval} for --enable-qv4l2) ;;
   esac]
)

AC_ARG_ENABLE(qvidcap,
  AS_HELP_STRING([--disable-qvidcap], [disable qvidcap compilation]),
  [case "${enableval}" in
     yes | no ) ;;
     *) AC_MSG_ERROR(bad value ${enableval} for --enable-qvidcap) ;;
   esac]
)

AC_ARG_ENABLE(gconv,
  AS_HELP_STRING([--enable-gconv], [enable compilation of gconv modules]),
  [case "${enableval}" in
    yes | no ) ;;
    *) AC_MSG_ERROR(bad value ${enableval} for --enable-gconv) ;;
   esac]
)

AC_ARG_ENABLE(bpf,
  AS_HELP_STRING([--disable-bpf], [disable IR BPF decoders]),
  [case "${enableval}" in
    yes | no ) ;;
    *) AC_MSG_ERROR(bad value ${enableval} for --enable-bpf) ;;
   esac]
)

PKG_CHECK_MODULES([SDL2], [sdl2 SDL2_image], [sdl_pc=yes], [sdl_pc=no])
AM_CONDITIONAL([HAVE_SDL], [test x$sdl_pc = xyes])

# Check if backtrace functions are defined
AC_SEARCH_LIBS([backtrace], [execinfo], [
  AC_DEFINE(HAVE_BACKTRACE, [1], [glibc has functions to provide stack backtrace])
])

AM_CONDITIONAL([WITH_LIBDVBV5],     [test x$enable_libdvbv5  != xno -a x$have_libudev = xyes])
AM_CONDITIONAL([WITH_DVBV5_REMOTE], [test x$enable_libdvbv5  != xno -a x$have_libudev = xyes -a x$have_pthread = xyes])

AM_CONDITIONAL([WITH_DYN_LIBV4L],   [test x$enable_dyn_libv4l != xno])
AM_CONDITIONAL([WITH_V4LUTILS],	    [test x$enable_v4l_utils != xno -a x$linux_os = xyes])
AM_CONDITIONAL([WITH_QV4L2],	    [test x${qt_pkgconfig} = xtrue -a x$enable_qv4l2 != xno])
AM_CONDITIONAL([WITH_QVIDCAP],	    [test x${qt_desktop_opengl} = xyes -a x$enable_qvidcap != xno])
AM_CONDITIONAL([WITH_V4L_PLUGINS],  [test x$enable_dyn_libv4l != xno -a x$enable_shared != xno])
AM_CONDITIONAL([WITH_V4L_WRAPPERS], [test x$enable_dyn_libv4l != xno -a x$enable_shared != xno])
AM_CONDITIONAL([WITH_QTGL],	    [test x${qt_desktop_opengl} = xyes])
AM_CONDITIONAL([WITH_GCONV],        [test x$enable_gconv = xyes -a x$enable_shared == xyes -a x$with_gconvdir != x -a -f $with_gconvdir/gconv-modules])
AM_CONDITIONAL([WITH_V4L2_CTL_LIBV4L], [test x${enable_v4l2_ctl_libv4l} != xno])
AM_CONDITIONAL([WITH_V4L2_CTL_STREAM_TO], [test x${enable_v4l2_ctl_stream_to} != xno])
AM_CONDITIONAL([WITH_V4L2_COMPLIANCE], [test x$ac_cv_func_fork = xyes])
AM_CONDITIONAL([WITH_V4L2_COMPLIANCE_LIBV4L], [test x$ac_cv_func_fork = xyes -a x${enable_v4l2_compliance_libv4l} != xno])
AM_CONDITIONAL([WITH_BPF],          [test x$enable_bpf != xno -a x$libelf_pkgconfig = xyes -a x$CLANG = xclang])

# append -static to libtool compile and link command to enforce static libs
AS_IF([test x$enable_libdvbv5 = xno], [AC_SUBST([ENFORCE_LIBDVBV5_STATIC], ["-static"])])
AS_IF([test x$enable_libv4l = xno],   [AC_SUBST([ENFORCE_LIBV4L_STATIC],   ["-static"])])

# misc

if test "x$linux_os" = "xyes"; then
  CPPFLAGS="-I\$(top_srcdir)/include $CPPFLAGS"
elif test "x$freebsd_os" = "xyes"; then
  CPPFLAGS="-I\$(top_srcdir)/contrib/freebsd/include $CPPFLAGS"
fi

CPPFLAGS="-I\$(top_srcdir)/lib/include -Wall -Wpointer-arith -D_GNU_SOURCE $CPPFLAGS"

AM_COND_IF([WITH_LIBDVBV5], [USE_LIBDVBV5="yes"], [USE_LIBDVBV5="no"])
AM_COND_IF([WITH_DVBV5_REMOTE], [USE_DVBV5_REMOTE="yes"
				 AC_DEFINE([HAVE_DVBV5_REMOTE], [1], [Usage of DVBv5 remote enabled])],
			        [USE_DVBV5_REMOTE="no"])
AM_COND_IF([WITH_DYN_LIBV4L], [USE_DYN_LIBV4L="yes"], [USE_DYN_LIBV4L="no"])
AM_COND_IF([WITH_V4LUTILS], [USE_V4LUTILS="yes"], [USE_V4LUTILS="no"])
AM_COND_IF([WITH_QV4L2], [USE_QV4L2="yes"], [USE_QV4L2="no"])
AM_COND_IF([WITH_QVIDCAP], [USE_QVIDCAP="yes"], [USE_QVIDCAP="no"])
AM_COND_IF([WITH_V4L_PLUGINS], [USE_V4L_PLUGINS="yes"
				AC_DEFINE([HAVE_V4L_PLUGINS], [1], [V4L plugin support enabled])],
				[USE_V4L_PLUGINS="no"])
AM_COND_IF([WITH_V4L_WRAPPERS], [USE_V4L_WRAPPERS="yes"], [USE_V4L_WRAPPERS="no"])
AM_COND_IF([WITH_GCONV], [USE_GCONV="yes"], [USE_GCONV="no"])
AM_COND_IF([WITH_V4L2_CTL_LIBV4L], [USE_V4L2_CTL_LIBV4L="yes"], [USE_V4L2_CTL_LIBV4L="no"])
AM_COND_IF([WITH_V4L2_COMPLIANCE], [USE_V4L2_COMPLIANCE="yes"], [USE_V4L2_COMPLIANCE="no"])
AM_COND_IF([WITH_V4L2_COMPLIANCE_LIBV4L], [USE_V4L2_COMPLIANCE_LIBV4L="yes"], [USE_V4L2_COMPLIANCE_LIBV4L="no"])
AM_COND_IF([WITH_BPF],         [USE_BPF="yes"
                                AC_DEFINE([HAVE_BPF], [1], [BPF IR decoder support enabled])],
				[USE_BPF="no"])
AS_IF([test "x$alsa_pkgconfig" = "xtrue"], [USE_ALSA="yes"], [USE_ALSA="no"])

AC_OUTPUT

if test x$enable_gconv = xyes -a x$USE_GCONV = xno; then
	echo
	echo "Failed to enable gconv. Disabling it"
fi

dnl ---------------------------------------------------------------------
dnl compile time options summary

cat <<EOF

compile time options summary
============================

    Host OS                    : $host_os
    X11                        : $x11_pkgconfig
    GL                         : $gl_pkgconfig
    glu                        : $glu_pkgconfig
    libelf		       : $libelf_pkgconfig
    libjpeg                    : $have_jpeg
    libudev                    : $have_libudev
    pthread                    : $have_pthread
    QT version                 : $QT_VERSION
    ALSA support               : $USE_ALSA
    SDL support		       : $sdl_pc

    build dynamic libs         : $enable_shared
    build static libs          : $enable_static

    gconv                      : $USE_GCONV

    dynamic libv4l             : $USE_DYN_LIBV4L
    v4l_plugins                : $USE_V4L_PLUGINS
    v4l_wrappers               : $USE_V4L_WRAPPERS
    libdvbv5                   : $USE_LIBDVBV5
    dvbv5-daemon               : $USE_DVBV5_REMOTE
    v4lutils                   : $USE_V4LUTILS
    qv4l2                      : $USE_QV4L2
    qvidcap                    : $USE_QVIDCAP
    v4l2-ctl uses libv4l       : $USE_V4L2_CTL_LIBV4L
    v4l2-compliance            : $USE_V4L2_COMPLIANCE
    v4l2-compliance uses libv4l: $USE_V4L2_COMPLIANCE_LIBV4L
    BPF IR Decoders:           : $USE_BPF
EOF
