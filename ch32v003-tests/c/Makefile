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

# firmware-test files

FW_TEST_SRCS := start.c main.c
FW_TEST_OBJS := $(FW_TEST_SRCS:.c=.o)
FW_TEST_LDS := ld/flash.ld

firmware-test: $(FW_TEST_LDS) $(FW_TEST_OBJS)
	$(LD) -T$< $(LDFLAGS) $(filter-out $<, $^) -o $@.elf
	$(CP) -O binary $@.elf $@.bin

# common

all: firmware-test

%.o: %.s
	$(AS) $(ASFLAGS) $< -o $@

%.o: %.c
	$(CC) $(OPTS) $(CCFLAGS) -c $< -o $@

clean:
	rm -rf *.o firmware-*.*

.PHONY: all run clean
