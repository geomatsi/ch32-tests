#
#
#

CROSS_COMPILE ?= riscv32-unknown-elf-

CP = $(CROSS_COMPILE)objcopy
CC = $(CROSS_COMPILE)gcc
AS = $(CROSS_COMPILE)as
LD = $(CROSS_COMPILE)ld

ASFLAGS = -march=rv32e_zicsr
CCFLAGS = -march=rv32e_zicsr -mabi=ilp32e -std=c11 -ffunction-sections -fdata-sections -ffreestanding -Wall -Wextra -Werror -fno-stack-protector
LDFLAGS = -nostdlib --gc-sections --fatal-warnings

VPATH += src

# test1 files

TEST1_SRCS := start.c t1.c
TEST1_OBJS := $(TEST1_SRCS:.c=.o)
TEST1_LDS := ld/flash.ld

test1: $(TEST1_LDS) $(TEST1_OBJS)
	$(LD) -T$< $(LDFLAGS) $(filter-out $<, $^) -o $@.elf
	$(CP) -O binary $@.elf $@.bin

# common

all: test1

%.o: %.s
	$(AS) $(ASFLAGS) $< -o $@

%.o: %.c
	$(CC) $(OPTS) $(CCFLAGS) -c $< -o $@

clean:
	rm -rf *.o
	rm -rf test*

.PHONY: all run clean
