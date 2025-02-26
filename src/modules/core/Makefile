CFLAGS += -I../..

LDFLAGS += -L../../framework -lmlt -lm -lpthread

include ../../../config.mak

TARGET = ../libmltcore$(LIBSUF)

OBJS = factory.o \
	   producer_colour.o \
	   producer_consumer.o \
	   producer_hold.o \
	   producer_loader.o \
	   producer_melt.o \
	   producer_noise.o \
	   producer_timewarp.o \
	   producer_tone.o \
	   filter_audiochannels.o \
	   filter_audiomap.o \
	   filter_audioconvert.o \
	   filter_audiowave.o \
	   filter_brightness.o \
	   filter_channelcopy.o \
	   filter_choppy.o \
	   filter_crop.o \
	   filter_fieldorder.o \
	   filter_gamma.o \
	   filter_greyscale.o \
	   filter_imageconvert.o \
	   filter_luma.o \
       filter_mask_apply.o \
       filter_mask_start.o \
	   filter_mirror.o \
	   filter_mono.o \
	   filter_obscure.o \
	   filter_panner.o \
	   filter_rescale.o \
	   filter_resize.o \
	   filter_transition.o \
	   filter_watermark.o \
	   link_timeremap.o \
	   transition_composite.o \
	   transition_luma.o \
	   transition_mix.o \
	   transition_matte.o \
	   consumer_multi.o \
	   consumer_null.o

ifdef SSE2_FLAGS
ifdef ARCH_X86_64
OBJS += composite_line_yuv_sse2_simple.o
endif
endif

ASM_OBJS = 

SRCS := $(OBJS:.o=.c)

ifeq ($(targetos), MinGW)
CFLAGS += -I../../win32
OBJS += ../../win32/fnmatch.o
SRCS += ../../win32/fnmatch.c
endif

all: 	$(TARGET)

$(TARGET): $(OBJS) $(ASM_OBJS)
	$(CC) $(SHFLAGS) -o $@ $(OBJS) $(ASM_OBJS) $(LDFLAGS)

composite_line_yuv_mmx.o: composite_line_yuv_mmx.S
	$(CC) -o $@ -c composite_line_yuv_mmx.S

depend:	$(SRCS)
	$(CC) -MM $(CFLAGS) $^ 1>.depend

distclean:	clean
	rm -f .depend

clean:	
	rm -f $(OBJS) $(ASM_OBJS) $(TARGET)

install: all
	install -m 755 $(TARGET) "$(DESTDIR)$(moduledir)"
	install -d "$(DESTDIR)$(mltdatadir)/core"
	install -m 644 loader.dict "$(DESTDIR)$(mltdatadir)/core"
	install -m 644 loader.ini "$(DESTDIR)$(mltdatadir)/core"
	install -m 644 *.yml "$(DESTDIR)$(mltdatadir)/core"

ifneq ($(wildcard .depend),)
include .depend
endif
