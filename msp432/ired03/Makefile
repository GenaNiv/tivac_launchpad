
#ARMGNU ?= arm-none-eabi
ARMGNU ?= arm-linux-gnueabi

AOPS = --warn --fatal-warnings -mcpu=cortex-m0
COPS = -Wall -Werror -O2 -nostdlib -nostartfiles -ffreestanding  -mcpu=cortex-m0

LOPS = -Wall -m32 -emit-llvm -target arm-none-eabi -mcpu=cortex-m0 -mthumb
LLCOPS = -march=thumb -mcpu=cortex-m0
#LLCOPS = -mcpu=cortex-m0
COPS = -Wall  -O2 -nostdlib -nostartfiles -ffreestanding
OOPS = -std-compile-opts

gcc : ired03.gcc.thumb.sram.bin

all : gcc clang

clang : ired03.clang.thumb.norm.flash.bin ired03.clang.thumb.opt.flash.bin ired03.clang.thumb.norm.sram.bin ired03.clang.thumb.opt.sram.bin 

clean:
	rm -f *.bin
	rm -f *.o
	rm -f *.elf
	rm -f *.list
	rm -f *.bc
	rm -f *.opt.s
	rm -f *.norm.s

#---------------------------------

flash.o : flash.s
	$(ARMGNU)-as $(AOPS) flash.s -o flash.o

sram.o : sram.s
	$(ARMGNU)-as $(AOPS) sram.s -o sram.o

ired03.gcc.thumb.o : ired03.c
	$(ARMGNU)-gcc $(COPS) -mthumb -c ired03.c -o ired03.gcc.thumb.o

ired03.gcc.thumb.flash.bin : flash.ld flash.o ired03.gcc.thumb.o
	$(ARMGNU)-ld -o ired03.gcc.thumb.flash.elf -T flash.ld flash.o ired03.gcc.thumb.o
	$(ARMGNU)-objdump -D ired03.gcc.thumb.flash.elf > ired03.gcc.thumb.flash.list
	$(ARMGNU)-objcopy ired03.gcc.thumb.flash.elf ired03.gcc.thumb.flash.bin -O binary

ired03.gcc.thumb.sram.bin : sram.ld sram.o ired03.gcc.thumb.o
	$(ARMGNU)-ld -o ired03.gcc.thumb.sram.elf -T sram.ld sram.o ired03.gcc.thumb.o
	$(ARMGNU)-objdump -D ired03.gcc.thumb.sram.elf > ired03.gcc.thumb.sram.list
	$(ARMGNU)-objcopy ired03.gcc.thumb.sram.elf ired03.gcc.thumb.sram.bin -O binary

#---------------------------------

ired03.clang.bc : ired03.c
	clang $(LOPS) -c ired03.c -o ired03.clang.bc

ired03.clang.thumb.norm.flash.bin : flash.ld flash.o ired03.clang.bc
	#llc $(LLCOPS) ired03.clang.bc -o ired03.clang.thumb.norm.s
	#$(ARMGNU)-as $(AOPS) ired03.clang.thumb.norm.s -o ired03.clang.thumb.norm.o
	llc $(LLCOPS) ired03.clang.bc -filetype=obj -o ired03.clang.thumb.norm.o
	$(ARMGNU)-ld -o ired03.clang.thumb.norm.flash.elf -T flash.ld flash.o ired03.clang.thumb.norm.o
	$(ARMGNU)-objdump -D ired03.clang.thumb.norm.flash.elf > ired03.clang.thumb.norm.flash.list
	$(ARMGNU)-objcopy ired03.clang.thumb.norm.flash.elf ired03.clang.thumb.norm.flash.bin -O binary

ired03.clang.thumb.opt.flash.bin : flash.ld flash.o ired03.clang.bc
	opt $(OOPS) ired03.clang.bc -o ired03.clang.thumb.opt.bc
	#llc $(LLCOPS) ired03.clang.thumb.opt.bc -o ired03.clang.thumb.opt.s
	#$(ARMGNU)-as $(AOPS) ired03.clang.thumb.opt.s -o ired03.clang.thumb.opt.o
	llc $(LLCOPS) ired03.clang.thumb.opt.bc -filetype=obj -o ired03.clang.thumb.opt.o
	$(ARMGNU)-ld -o ired03.clang.thumb.opt.flash.elf -T flash.ld flash.o ired03.clang.thumb.opt.o
	$(ARMGNU)-objdump -D ired03.clang.thumb.opt.flash.elf > ired03.clang.thumb.opt.flash.list
	$(ARMGNU)-objcopy ired03.clang.thumb.opt.flash.elf ired03.clang.thumb.opt.flash.bin -O binary


ired03.clang.thumb.norm.sram.bin : sram.ld sram.o ired03.clang.bc
	#llc $(LLCOPS) ired03.clang.bc -o ired03.clang.thumb.norm.s
	#$(ARMGNU)-as $(AOPS) ired03.clang.thumb.norm.s -o ired03.clang.thumb.norm.o
	llc $(LLCOPS) ired03.clang.bc -filetype=obj -o ired03.clang.thumb.norm.o
	$(ARMGNU)-ld -o ired03.clang.thumb.norm.sram.elf -T sram.ld sram.o ired03.clang.thumb.norm.o
	$(ARMGNU)-objdump -D ired03.clang.thumb.norm.sram.elf > ired03.clang.thumb.norm.sram.list
	$(ARMGNU)-objcopy ired03.clang.thumb.norm.sram.elf ired03.clang.thumb.norm.sram.bin -O binary

ired03.clang.thumb.opt.sram.bin : sram.ld sram.o ired03.clang.bc
	opt $(OOPS) ired03.clang.bc -o ired03.clang.thumb.opt.bc
	#llc $(LLCOPS) ired03.clang.thumb.opt.bc -o ired03.clang.thumb.opt.s
	#$(ARMGNU)-as $(AOPS) ired03.clang.thumb.opt.s -o ired03.clang.thumb.opt.o
	llc $(LLCOPS) ired03.clang.thumb.opt.bc -filetype=obj -o ired03.clang.thumb.opt.o
	$(ARMGNU)-ld -o ired03.clang.thumb.opt.sram.elf -T sram.ld sram.o ired03.clang.thumb.opt.o
	$(ARMGNU)-objdump -D ired03.clang.thumb.opt.sram.elf > ired03.clang.thumb.opt.sram.list
	$(ARMGNU)-objcopy ired03.clang.thumb.opt.sram.elf ired03.clang.thumb.opt.sram.bin -O binary




