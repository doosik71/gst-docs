CC=gcc
CFLAGS=`pkg-config --cflags --libs gstreamer-video-1.0 gtk+-3.0 gstreamer-1.0 gstreamer-audio-1.0 gstreamer-pbutils-1.0 clutter-gst-3.0`

all: \
	basic-tutorial-1.o \
	basic-tutorial-2.o \
	basic-tutorial-3.o \
	basic-tutorial-4.o \
	basic-tutorial-5.o \
	basic-tutorial-6.o \
	basic-tutorial-7.o \
	basic-tutorial-8.o \
	basic-tutorial-9.o \
	basic-tutorial-12.o \
	basic-tutorial-13.o \
	basic-tutorial-15.o

.PHONY: all

%.o: %.c
	gcc -o $@ $^ $(CFLAGS)
