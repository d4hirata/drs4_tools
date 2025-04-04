########################################################
#
#  Makefile for drscl, drscmd, drs_exam, drs_exam_multi
#  (drsosc を除外)
#
#  Requires wxWidgets 2.8.9 or newer
#  Modified by mzks (Nov. 12 2020)
#  Further modified to disable drsosc and add install
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

ifeq ($(OS),Darwin)
CFLAGS        += -stdlib=libc++
CFLAGS        += -I/opt/homebrew/include
endif         

# wxWidgets libs and flags
WXLIBS        = $(shell wx-config --libs)
WXFLAGS       = $(shell wx-config --cxxflags)

CPP_OBJ       = DRS.o averager.o ConfigDialog.o DOFrame.o DOScreen.o DRSOsc.o MeasureDialog.o Measurement.o Osci.o InfoDialog.o DisplayDialog.o AboutDialog.o EPThread.o TriggerDialog.o rb.o
OBJECTS       = musbstd.o mxml.o strlcpy.o

# install path
PREFIX = /usr/local

# Build only drscl, drscmd, drs_exam, drs_exam_multi
all: drscl drscmd drs_exam drs_exam_multi

# Install target
install: drscl drscmd
	install -d $(PREFIX)/bin
	install drscl $(PREFIX)/bin/
	install drscmd $(PREFIX)/bin/

# Individual targets
drscl: $(OBJECTS) DRS.o averager.o drscl.o
	$(CXX) $(CFLAGS) $(OBJECTS) DRS.o averager.o drscl.o -o drscl $(LIBS) $(WXLIBS)

drscmd: $(OBJECTS) DRS.o averager.o drscmd.o
	$(CXX) $(CFLAGS) $(OBJECTS) DRS.o averager.o drscmd.o -o drscmd $(LIBS) $(WXLIBS)

drs_exam: $(OBJECTS) DRS.o averager.o drs_exam.o
	$(CXX) $(CFLAGS) $(OBJECTS) DRS.o averager.o drs_exam.o -o drs_exam $(LIBS) $(WXLIBS)

drs_exam_multi: $(OBJECTS) DRS.o averager.o drs_exam_multi.o
	$(CXX) $(CFLAGS) $(OBJECTS) DRS.o averager.o drs_exam_multi.o -o drs_exam_multi $(LIBS) $(WXLIBS)

# Object rules
main.o: src/main.cpp include/mxml.h include/DRS.h
	$(CXX) $(CFLAGS) $(WXFLAGS) -c $<

drscl.o: src/drscl.cpp include/mxml.h include/DRS.h
	$(CXX) $(CFLAGS) -c $<

drscmd.o: src/drscmd.cpp include/mxml.h include/DRS.h
	$(CXX) $(CFLAGS) -c $<

drs_exam.o: src/drs_exam.cpp include/mxml.h include/DRS.h
	$(CXX) $(CFLAGS) -c $<

drs_exam_multi.o: src/drs_exam_multi.cpp include/mxml.h include/DRS.h
	$(CXX) $(CFLAGS) -c $<

$(CPP_OBJ): %.o: src/%.cpp include/%.h include/mxml.h include/DRS.h
	$(CXX) $(CFLAGS) $(WXFLAGS) -c $<

$(OBJECTS): %.o: src/%.c include/mxml.h include/DRS.h
	$(CC) $(CFLAGS) -c $<

# Clean target
clean:
	rm -f *.o drscl drsosc drscmd drs_exam drs_exam_multi
