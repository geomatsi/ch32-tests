#
#
#

CROSS_COMPILE ?= riscv32-unknown-elf-
CH32V003EVT ?= /path/to/CH32V003EVT

CP = $(CROSS_COMPILE)objcopy
CC = $(CROSS_COMPILE)gcc
AS = $(CROSS_COMPILE)as
LD = $(CROSS_COMPILE)ld

ASFLAGS = -march=rv32e_zicsr
CCFLAGS = -march=rv32e_zicsr -mabi=ilp32e -std=gnu99 -ffunction-sections -fdata-sections -ffreestanding -Wall -Wextra -Werror -fno-stack-protector
LDFLAGS = -nostdlib --gc-sections --fatal-warnings

CCFLAGS += -Ievt/include \
	   -I$(CH32V003EVT)/EXAM/SRC/Peripheral/inc \
	   -I$(CH32V003EVT)/EXAM/SRC/Core

VPATH += src \
	 evt/src \
	 $(CH32V003EVT)/EXAM/SRC/Peripheral/src

# test1 files

TEST1_SRCS := start.c t1.c
TEST1_OBJS := $(TEST1_SRCS:.c=.o)
TEST1_LDS := ld/flash.ld

test1: $(TEST1_LDS) $(TEST1_OBJS)
	$(LD) -T$< $(LDFLAGS) $(filter-out $<, $^) -o $@.elf
	$(CP) -O binary $@.elf $@.bin

# test2 files

TEST2_SRCS := start.c t2.c
TEST2_OBJS := $(TEST2_SRCS:.c=.o)
TEST2_LDS := ld/flash.ld

test2: $(TEST2_LDS) $(TEST2_OBJS)
	$(LD) -T$< $(LDFLAGS) $(filter-out $<, $^) -o $@.elf
	$(CP) -O binary $@.elf $@.bin

# test3 files

TEST3_SRCS := start.c t3.c ch32v00x_rcc.c ch32v00x_gpio.c
TEST3_OBJS := $(TEST3_SRCS:.c=.o)
TEST3_LDS := ld/flash.ld

test3: $(TEST3_LDS) $(TEST3_OBJS)
	$(LD) -T$< $(LDFLAGS) $(filter-out $<, $^) -o $@.elf
	$(CP) -O binary $@.elf $@.bin

# test4 files

TEST4_SRCS := start.c t4.c ch32v00x_rcc.c ch32v00x_gpio.c system_ch32v00x.c
TEST4_OBJS := $(TEST4_SRCS:.c=.o)
TEST4_LDS := ld/flash.ld

test4: $(TEST4_LDS) $(TEST4_OBJS)
	$(LD) -T$< $(LDFLAGS) $(filter-out $<, $^) -o $@.elf
	$(CP) -O binary $@.elf $@.bin


# common

all: test1 test2 test3

%.o: %.s
	$(AS) $(ASFLAGS) $< -o $@

%.o: %.c
	$(CC) $(OPTS) $(CCFLAGS) -c $< -o $@

clean:
	rm -rf *.o
	rm -rf test*

.PHONY: all run clean
