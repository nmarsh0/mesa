

# Copyright © 2011 Intel Corporation
#
# Permission is hereby granted, free of charge, to any person obtaining a
# copy of this software and associated documentation files (the "Software"),
# to deal in the Software without restriction, including without limitation
# the rights to use, copy, modify, merge, publish, distribute, sublicense,
# and/or sell copies of the Software, and to permit persons to whom the
# Software is furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice (including the next
# paragraph) shall be included in all copies or substantial portions of the
# Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.  IN NO EVENT SHALL
# THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
# FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
# IN THE SOFTWARE.

include Makefile.sources

# Hack to make some of the non-automake variables work.
TOP=$(top_builddir)

AM_CFLAGS = \
	-I$(top_srcdir)/include \
	-I$(top_srcdir)/src/ \
	-I$(top_srcdir)/src/mapi \
	-I$(top_srcdir)/src/mesa/ \
	-I../common \
	-I../intel \
	-I../intel/server \
	$(DEFINES) \
	$(ASM_FLAGS) \
	$(API_DEFINES) \
	$(INTEL_CFLAGS)

AM_CXXFLAGS = $(AM_CFLAGS)

dridir = $(DRI_DRIVER_INSTALL_DIR)

if HAVE_I965_DRI
dri_LTLIBRARIES = i965_dri.la
endif

# These should be an ltlibrary that's built once.  Let's fix that once
# the other drivers are converted to automake.
COMMON_FILES = \
	../common/utils.c \
	../common/dri_util.c \
	../common/xmlconfig.c

i965_dri_la_SOURCES = \
	$(i965_C_FILES) \
	$(i965_CXX_FILES) \
	$(COMMON_FILES)

i965_dri_la_LDFLAGS = -module -avoid-version -shared
i965_dri_la_LIBADD = \
	$(DRI_LIB_DEPS) \
	$(DRICORE_LIB_DEPS) \
	$(INTEL_LIBS)

# Test build to make sure that we didn't leave any undefined symbols in the
# driver.
noinst_PROGRAMS = i965_symbols_test

i965_symbols_test_SOURCES = \
	../common/dri_test.c
i965_symbols_test_LDADD = \
	i965_dri.la

# Provide compatibility with scripts for the old Mesa build system for
# a while by putting a link to the driver into /lib of the build tree.
all-local: i965_dri.la
	$(MKDIR_P) $(top_builddir)/$(LIB_DIR);
	ln -f .libs/i965_dri.so $(top_builddir)/$(LIB_DIR)/i965_dri.so;