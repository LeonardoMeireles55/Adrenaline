TARGET = satelite
OBJS = main.o menu.o blit.o scePaf.o exports.o

INCDIR = ../include
CFLAGS = -O2 -Os -G0 -Wall -fshort-wchar -fno-pic -mno-check-zero-division
CXXFLAGS = $(CFLAGS) -fno-exceptions -fno-rtti
ASFLAGS = $(CFLAGS)

BUILD_PRX = 1
PRX_EXPORTS = exports.exp

USE_PSPSDK_LIBS = 0
USE_PSPSDK_LIBC = 0

LIBDIR = ../lib
LIBS = -lpspsystemctrl_user -lpspvshctrl -lpsppower

PSPSDK = $(shell psp-config --pspsdk-path)
include $(PSPSDK)/lib/build_prx.mak