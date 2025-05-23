# Copyright (c) 2013
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
#
#     * Redistributions of source code must retain the above copyright notice,
#       this list of conditions and the following disclaimer.
#     * Redistributions in binary form must reproduce the above copyright
#       notice, this list of conditions and the following disclaimer in the
#       documentation and/or other materials provided with the distribution.
#     * Neither the name of HyperLevelDB nor the names of its contributors may
#       be used to endorse or promote products derived from this software
#       without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
# ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE
# LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
# CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
# SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
# INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
# CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
# ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.
#
#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

AC_PREREQ([2.62])
AC_INIT([hyperleveldb], [1.2.2], [robert@hyperdex.org])
m4_define([serial_tests], [
    m4_esyscmd([case `automake --version | head -n 1` in
                *1.11*);;
                *) echo serial-tests;;
                esac])
])
AM_INIT_AUTOMAKE(foreign serial_tests subdir-objects dist-bzip2)
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])
LT_PREREQ([2.2])
LT_INIT
AC_CONFIG_SRCDIR([include/hyperleveldb/db.h])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([m4])

# Checks for programs.
AC_PROG_CXX
AC_PROG_CC
AC_LANG(C++)

ANAL_WARNINGS

EXTRA_CFLAGS=""
AX_CHECK_COMPILE_FLAG([-fno-builtin-memcmp],[EXTRA_CFLAGS="-fno-builtin-memcmp"],,)
AX_CHECK_COMPILE_FLAG([-fno-builtin-memmove],[EXTRA_CFLAGS="${EXTRA_CFLAGS} -fno-builtin-memcmp"],,)
AX_CHECK_COMPILE_FLAG([-Qunused-arguments],[EXTRA_CFLAGS="${EXTRA_CFLAGS} -Qunused-arguments"],,)
#AX_CHECK_COMPILE_FLAG([-Wno-unknown-warning-option],[EXTRA_CFLAGS="${EXTRA_CFLAGS} -Wno-unknown-warning-option"],,)
AC_SUBST([EXTRA_CFLAGS], [${EXTRA_CFLAGS}])

# Checks for libraries.

# Checks for header files.
AC_CHECK_HEADERS([endian.h machine/endian.h sys/endian.h sys/isa_defs.h sys/types.h])

# Checks for typedefs, structures, and compiler characteristics.

# Checks for library functions.
AC_FUNC_ERROR_AT_LINE
AC_FUNC_MMAP
AC_CHECK_FUNCS([alarm clock_gettime mach_absolute_time ftruncate fsync])
AC_CHECK_FUNCS([fread fread_unlocked fwrite fwrite_unlocked fflush])
AC_CHECK_FUNCS([fflush_unlocked memmove mkdir munmap rmdir socket])
AC_CHECK_DECLS([fdatasync])

# Optional components
snappy_detect_hdr=yes
snappy_detect_lib=yes
AC_CHECK_LIB([snappy], [snappy_compress], [snappy_detect_hdr=yes], [snappy_detect_hdr=no])
AC_CHECK_HEADER([snappy.h],,[snappy_detect_lib=no])
AC_ARG_ENABLE([snappy], [AS_HELP_STRING([--enable-snappy],
              [build with Snappy @<:@default: auto@:>@])],
              [snappy=${enableval}], [snappy=auto])
if test x"${snappy}" = xno; then
snappy_detect_hdr=no
snappy_detect_lib=no
fi
if test x"${snappy}" = xyes; then
    if test x"${snappy_detect_hdr}" != xyes; then
        AC_MSG_ERROR([
-------------------------------------------------
LevelDB configured with the Snappy library.
libsnappy.so not found
Please install Snappy to continue.
-------------------------------------------------])
    fi
    if test x"${snappy_detect_lib}" != xyes; then
        AC_MSG_ERROR([
-------------------------------------------------
LevelDB configured with the Snappy library.
snappy.h not found
Please install Snappy to continue.
-------------------------------------------------])
    fi
fi
if test x"${snappy_detect_hdr}" = xyes -a x"${snappy_detect_lib}" = xyes; then
SNAPPY_FLAGS=-DSNAPPY
SNAPPY_LIBS=-lsnappy
else
SNAPPY_FLAGS=
SNAPPY_LIBS=
fi
AC_SUBST(SNAPPY_FLAGS)
AC_SUBST(SNAPPY_LIBS)

AC_CONFIG_FILES([Makefile libhyperleveldb.pc hyperleveldb.upack])
AC_OUTPUT
