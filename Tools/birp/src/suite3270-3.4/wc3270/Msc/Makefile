# Copyright (c) 2007-2015, Paul Mattes.
# All rights reserved.
# 
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#     * Redistributions of source code must retain the above copyright
#       notice, this list of conditions and the following disclaimer.
#     * Redistributions in binary form must reproduce the above copyright
#       notice, this list of conditions and the following disclaimer in the
#       documentation and/or other materials provided with the distribution.
#     * Neither the name of Paul Mattes nor his contributors may be used
#       to endorse or promote products derived from this software without
#       specific prior written permission.
# 
# THIS SOFTWARE IS PROVIDED BY PAUL MATTES "AS IS" AND ANY EXPRESS OR
# IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES
# OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN
# NO EVENT SHALL PAUL MATTES BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
# SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED
# TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR
# PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF
# LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
# NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
# SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

# nmake Makefile for wc3270

# Set command paths.
MKFB = mkfb.exe
MKKEYPAD = mkkeypad.exe
CC = cl /nologo
RC = rc /nologo
RM = erase

# This makefile assumes that you have OpenSSL headers in c:\OpenSSL-Win32.
# If they are somewhere else, change SSLDIR below.
SSLDIR = C:\OpenSSL-Win32
!IF EXISTS($(SSLDIR))
SSLDEF = /DHAVE_LIBSSL=1 /I$(SSLDIR)\include
SSLLIB = gdi32.lib
!ENDIF

LIB3270 = ..\lib\libw3270.lib
LIB3270I = ..\lib\libw3270i.lib
LIBSTUBS = ..\lib\libw3270stubs.lib
LIB32XX = ..\lib\libw32xx.lib

XCPPFLAGS = /D_WIN32 /D_WIN32_WINNT=0x0500 /D_WIN32_IE=0x0500 /DWINVER=0x500 /D_CRT_SECURE_NO_DEPRECATE /W3 /WX $(SSLDEF) -I. -I..\include
CFLAGS = $(EXTRA_FLAGS) $(XCPPFLAGS)

OBJ = obj
!INCLUDE common_files.mk
!INCLUDE wc3270_files.mk

VOBJS = $(COMMON_OBJECTS) $(HTTPD_OBJECTS) $(WC3270_OBJECTS) wc3270.RES \
	fallbacks.obj

OBJECTS = $(VOBJS) version.obj
WOBJECTS = wizard.obj wc3270.RES wversion.obj shortcut.obj relink.obj
LIBS = $(LIB3270) $(LIB3270I) $(LIB32XX) $(LIBSTUBS) $(SSLLIB) ws2_32.lib \
	advapi32.lib user32.lib shell32.lib ole32.lib comdlg32.lib gdi32.lib \
	winspool.lib
SHRTLIBS = $(LIB32XX) ole32.lib
WIZLIBS = $(LIB32XX) ole32.lib winspool.lib advapi32.lib shell32.lib

PROGS = wc3270.exe mkshort.exe wc3270wiz.exe catf.exe ead3270.exe
all: $(PROGS)

version.obj: $(VOBJS) version.txt mkversion.exe Msc\Makefile
	mkversion.exe
	$(CC) $(CFLAGS) /c version.c
mkversion.exe: Msc\mkversion.c
	$(CC) $(CFLAGS) /Fe$@ Msc\mkversion.c

FALLBACKS = fb-common fb-printSession fb-messages fb-composeMap fb-c3270
fallbacks.c: $(MKFB) $(FALLBACKS)
	$(MKFB) -c -o $@ $(FALLBACKS)
fallbacks.obj: fallbacks.c
	$(CC) $(CFLAGS) /c fallbacks.c

$(MKFB): mkfb.c conf.h
	$(CC) /Fe$(MKFB) $(XCPPFLAGS)  mkfb.c

compiled_keypad.h: keypad.full keypad.labels keypad.map keypad.outline keypad.callbacks $(MKKEYPAD)
	$(MKKEYPAD) >$@

$(MKKEYPAD): mkkeypad.c
	$(CC) /Fe$(MKKEYPAD) $(XCPPFLAGS)  mkkeypad.c

wc3270.RES: wc3270.rc wc3270.ico Msc\Makefile
	$(RC) wc3270.rc

wc3270.exe: $(OBJECTS) Msc\Makefile $(LIB3270) $(LIB3270I) $(LIB32XX) $(LIBSTUBS)
	$(CC) /Fewc3270.exe $(CFLAGS) $(OBJECTS) $(LIBS)

$(LIB3270):
	cd ..\lib\w3270 & $(MAKE) /nologo /fMsc\Makefile
$(LIB3270I):
	cd ..\lib\w3270i & $(MAKE) /nologo /fMsc\Makefile
$(LIB32XX):
	cd ..\lib\w32xx & $(MAKE) /nologo /fMsc\Makefile
$(LIBSTUBS):
	cd ..\lib\w3270stubs & $(MAKE) /nologo /fMsc\Makefile

mkshort.exe: mkshort.obj shortcut.obj $(LIB32XX) Msc\Makefile
	$(CC) /Femkshort.exe $(CFLAGS) mkshort.obj shortcut.obj $(SHRTLIBS)

wversion.obj: version.txt mkversion.exe
	mkversion.exe -w
	$(CC) $(CFLAGS) /c wversion.c

wc3270wiz.exe: $(WOBJECTS) $(LIB32XX)
	$(CC) /Fewc3270wiz.exe $(CFLAGS) $(WOBJECTS) $(WIZLIBS)

catf.exe: catf.c
	$(CC) /Fecatf.exe $(CFLAGS) catf.c

ead3270.exe: ead3270.obj $(LIB32XX)
	$(CC) /Fe$@ $(CFLAGS) ead3270.obj $(LIB32XX) advapi32.lib shell32.lib

clean:
	$(RM) *.obj *.RES *.exp *.lib $(MKFB) $(MKKEYPAD) compiled_keypad.h fallbacks.c mkversion.exe version.c wversion.c

clobber: clean
	$(RM) $(PROGS)

keypad.c: compiled_keypad.h
