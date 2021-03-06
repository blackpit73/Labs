# Copyright (c) 1999-2009, 2013-2015 Paul Mattes.
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
#     * Redistributions of source code must retain the above copyright
#       notice, this list of conditions and the following disclaimer.
#     * Redistributions in binary form must reproduce the above copyright
#       notice, this list of conditions and the following disclaimer in the
#       documentation and/or other materials provided with the distribution.
#     * Neither the names of Paul Mattes nor the names of his contributors
#       may be used to endorse or promote products derived from this software
#       without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY PAUL MATTES "AS IS" AND ANY EXPRESS OR IMPLIED
# WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
# MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
# EVENT SHALL PAUL MATTES BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
# SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
# PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
# OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
# WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
# OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF
# ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
#

# Makefile for 3270/3287 emulation common library (Windows)

LIB32XX = ../libw32xx.a

include winenv.mk

XCPPFLAGS = $(WIN32_FLAGS) -I. -I../include/windows -I../../include
CFLAGS = $(EXTRA_FLAGS) -g -Wall -Werror $(XCPPFLAGS) $(SSLCPP)

all:: $(LIB32XX)

OBJ = o
include lib32xx_files.mk
include libw32xx_files.mk
SRCS = $(LIB32XX_SOURCES) $(LIBW32XX_SOURCES)
OBJS = $(LIB32XX_OBJECTS) $(LIBW32XX_OBJECTS)

$(LIB32XX): $(OBJS)
	$(AR) crs $@ $(OBJS)

clean:
	$(RM) *.o

clobber: clean
	$(RM) $(LIB32XX)

depend:
	$(CC) -MM $(XCPPFLAGS) -I. $(SRCS) >dependencies-
	mv dependencies- dependencies

ifeq ($(wildcard dependencies),dependencies)
include dependencies
endif
