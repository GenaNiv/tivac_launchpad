
#ARMGNU=arm-none-linux-gnueabi
ARMGNU?=arm-none-eabi

LOPS = -Wall -m32 -emit-llvm -ccc-host-triple $(ARMGNU)

LLCOPS = -march=thumb -mtriple=$(ARMGNU)

LLCOPS2 = -march=thumb -mtriple=$(ARMGNU) -mcpu=cortex-m3

COPS = -Wall  -O2 -nostdlib -nostartfiles -ffreestanding

AOPS = --warn --fatal-warnings

OOPS = -std-compile-opts

stuff = -O2 -std-compile-opts -strip-debug



gcc : blinker01.gcc.thumb.bin blinker01.gcc.thumb2.bin 

llvm : blinker01.clang.thumb.norm.bin blinker01.clang.thumb.opt.bin blinker01.clang.thumb2.norm.bin blinker01.clang.thumb2.opt.bin

llvm-test : blinker01.clang.thumb2.test.bin

all : gcc llvm llvm-test

vectors.o : vectors.s
	$(ARMGNU)-as vectors.s -o vectors.o

blinker01.gcc.thumb.o : blinker01.c
	$(ARMGNU)-gcc $(COPS) -mthumb -c blinker01.c -o blinker01.gcc.thumb.o

blinker01.gcc.thumb2.o : blinker01.c
	$(ARMGNU)-gcc $(COPS) -mthumb -mcpu=cortex-m3 -march=armv7-m -c blinker01.c -o blinker01.gcc.thumb2.o

blinker01.gcc.thumb.bin : memmap vectors.o blinker01.gcc.thumb.o
	$(ARMGNU)-ld -o blinker01.gcc.thumb.elf -T memmap vectors.o blinker01.gcc.thumb.o
	$(ARMGNU)-objdump -D blinker01.gcc.thumb.elf > blinker01.gcc.thumb.list
	$(ARMGNU)-objcopy blinker01.gcc.thumb.elf blinker01.gcc.thumb.bin -O binary

blinker01.gcc.thumb2.bin : memmap vectors.o blinker01.gcc.thumb2.o
	$(ARMGNU)-ld -o blinker01.gcc.thumb2.elf -T memmap vectors.o blinker01.gcc.thumb2.o
	$(ARMGNU)-objdump -D blinker01.gcc.thumb2.elf > blinker01.gcc.thumb2.list
	$(ARMGNU)-objcopy blinker01.gcc.thumb2.elf blinker01.gcc.thumb2.bin -O binary


blinker01.clang.bc : blinker01.c
	clang $(LOPS) -c blinker01.c -o blinker01.clang.bc

blinker01.clang.thumb.norm.bin : memmap vectors.o blinker01.clang.bc
	llc $(LLCOPS) blinker01.clang.bc -o blinker01.clang.thumb.norm.s
	$(ARMGNU)-as blinker01.clang.thumb.norm.s -o blinker01.clang.thumb.norm.o
	$(ARMGNU)-ld -o blinker01.clang.thumb.norm.elf -T memmap vectors.o blinker01.clang.thumb.norm.o
	$(ARMGNU)-objdump -D blinker01.clang.thumb.norm.elf > blinker01.clang.thumb.norm.list
	$(ARMGNU)-objcopy blinker01.clang.thumb.norm.elf blinker01.clang.thumb.norm.bin -O binary

blinker01.clang.thumb.opt.bin : memmap vectors.o blinker01.clang.bc
	opt $(OOPS) blinker01.clang.bc -o blinker01.clang.thumb.opt.bc
	llc $(LLCOPS) blinker01.clang.thumb.opt.bc -o blinker01.clang.thumb.opt.s
	$(ARMGNU)-as blinker01.clang.thumb.opt.s -o blinker01.clang.thumb.opt.o
	$(ARMGNU)-ld -o blinker01.clang.thumb.opt.elf -T memmap vectors.o blinker01.clang.thumb.opt.o
	$(ARMGNU)-objdump -D blinker01.clang.thumb.opt.elf > blinker01.clang.thumb.opt.list
	$(ARMGNU)-objcopy blinker01.clang.thumb.opt.elf blinker01.clang.thumb.opt.bin -O binary

blinker01.clang.thumb2.norm.bin : memmap vectors.o blinker01.clang.bc
	llc $(LLCOPS2) blinker01.clang.bc -o blinker01.clang.thumb2.norm.s
	$(ARMGNU)-as blinker01.clang.thumb2.norm.s -o blinker01.clang.thumb2.norm.o
	$(ARMGNU)-ld -o blinker01.clang.thumb2.norm.elf -T memmap vectors.o blinker01.clang.thumb2.norm.o
	$(ARMGNU)-objdump -D blinker01.clang.thumb2.norm.elf > blinker01.clang.thumb2.norm.list
	$(ARMGNU)-objcopy blinker01.clang.thumb2.norm.elf blinker01.clang.thumb2.norm.bin -O binary

blinker01.clang.thumb2.opt.bin : memmap vectors.o blinker01.clang.bc
	opt $(OOPS) blinker01.clang.bc -o blinker01.clang.thumb2.opt.bc
	llc $(LLCOPS2) blinker01.clang.thumb2.opt.bc -o blinker01.clang.thumb2.opt.s
	$(ARMGNU)-as blinker01.clang.thumb2.opt.s -o blinker01.clang.thumb2.opt.o
	$(ARMGNU)-ld -o blinker01.clang.thumb2.opt.elf -T memmap vectors.o blinker01.clang.thumb2.opt.o
	$(ARMGNU)-objdump -D blinker01.clang.thumb2.opt.elf > blinker01.clang.thumb2.opt.list
	$(ARMGNU)-objcopy blinker01.clang.thumb2.opt.elf blinker01.clang.thumb2.opt.bin -O binary



blinker01.clang.thumb2.test.bin : memmap vectors.o blinker01.clang.bc
	opt $(OOPS) blinker01.clang.bc -o blinker01.clang.thumb2.test.bc
	llc $(LLCOPS2) -filetype=obj blinker01.clang.thumb2.test.bc -o blinker01.clang.thumb2.test.o
	$(ARMGNU)-ld -o blinker01.clang.thumb2.test.elf -T memmap vectors.o blinker01.clang.thumb2.test.o
	$(ARMGNU)-objdump -D blinker01.clang.thumb2.test.elf > blinker01.clang.thumb2.test.list
	$(ARMGNU)-objcopy blinker01.clang.thumb2.test.elf blinker01.clang.thumb2.test.bin -O binary







clean:
	rm -f *.bin
	rm -f *.o
	rm -f *.elf
	rm -f *.list
	rm -f *.bc
	rm -f *.opt.s
	rm -f *.norm.s

