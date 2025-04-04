########################################################
#
#  Makefile for drscl and drscmd (no wxWidgets)
#
#  Modified to remove dependency on wxWidgets
#  and drsosc-related targets
#
########################################################

CXX = /usr/bin/g++
CC = /usr/bin/gcc

# check for OS
OS            = $(shell uname)
ifeq ($(OS),Darwin)
DOS           = OS_DARWIN
else
DOS           = OS_LINUX
endif

CFLAGS        = -g -O2 -Wall -Wuninitialized -fno-strict-aliasing -Iinclude -I/usr/local/include -D$(DOS) -DHAVE_USB -DHAVE_LIBUSB10 -DUSE_DRS_MUTEX
LIBS          = -lpthread -lutil -lusb-1.0

# install path
PREFIX = /opt/drs4_tools

# Build only drscl and drscmd
all: drscl drscmd

install: drscl drscmd
	install -d $(PREFIX)/bin
	install drscl $(PREFIX)/bin/
	install drscmd $(PREFIX)/bin/

drscl: $(OBJECTS) DRS.o averager.o drscl.o
	$(CXX) $(CFLAGS) $(OBJECTS) DRS.o averager.o drscl.o -o drscl $(LIBS)

drscmd: $(OBJECTS) DRS.o averager.o drscmd.o
	$(CXX) $(CFLAGS) $(OBJECTS) DRS.o averager.o drscmd.o -o drscmd $(LIBS)

OBJECTS = musbstd.o mxml.o strlcpy.o

drscl.o: src/drscl.cpp include/mxml.h include/DRS.h
	$(CXX) $(CFLAGS) -c $<

drscmd.o: src/drscmd.cpp include/mxml.h include/DRS.h
	$(CXX) $(CFLAGS) -c $<

$(OBJECTS): %.o: src/%.c include/mxml.h include/DRS.h
	$(CC) $(CFLAGS) -c $<

averager.o: src/averager.cpp include/averager.h include/DRS.h
	$(CXX) $(CFLAGS) -c $<

DRS.o: src/DRS.cpp include/DRS.h
	$(CXX) $(CFLAGS) -c $<

clean:
	rm -f *.o drscl drscmd
