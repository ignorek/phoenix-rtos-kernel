#
# Makefile for Phoenix-RTOS kernel (vm)
#
# Copyright 2012-2015, 2017 Phoenix Systems
# Copyright 2001, 2005-2006 Pawel Pisarczyk
#

SRCS = proc.c threads.c process.c name.c resource.c mutex.c cond.c userintr.c file.c ports.c

ifneq (, $(findstring NOMMU, $(CFLAGS)))
	SRCS += msg-nommu.c
else
	SRCS += msg.c
endif

OBJS = $(SRCS:.c=.o)


all: check $(ARCH)


check:
	@(if test -z "$(ARCH)"; then\
		echo "Error: Only main Makefile can be used for compilation!";\
		exit 1;\
	fi;)


.c.o:
	@(printf " CC  %-24s  " "$<"; $(CC) -c $(CFLAGS) $<)

	@(file="$@"; \
	datasz=0;\
	textsz=0;\
	rodatasz=0;\
	for i in `$(OBJDUMP) -t $$file | grep -e " O " | grep -v ".rodata" | awk '{ print $$4 }'`; do \
		datasz=`echo $$(($$datasz + 0x$$i))`;\
	done; \
	for i in `$(OBJDUMP) -t $$file | grep -e " O " | grep ".rodata" | awk '{ print $$5 }'`; do \
                rodatasz=`echo $$(($$rodatasz + 0x$$i))`;\
        done; \
	for i in `$(OBJDUMP) -t $$file | grep -e "F" | awk '{ print $$5 }'`; do \
		textsz=`echo $$(($$textsz + 0x$$i))`;\
	done;\
	printf "data=%-5d  text=%-5d  rodata=%-5d\n" $$datasz $$textsz $$rodatasz)


-include .depend


$(ARCH): $(OBJS)
	$(SIL)$(AR) $(ARFLAGS) $(ARCH) $(OBJS) 2>/dev/null


depend: check
	$(SIL)$(MKDEP) $(MKDEPFLAGS) $(SRCS) >.depend


clean: check
	$(SIL)rm -f core *.o $(ARCH)


.PHONY: clean
# DO NOT DELETE
