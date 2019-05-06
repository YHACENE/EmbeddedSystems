# Makefile AVR
# Auteur : Hacene YOUNSI
# Email  : hacene.younsi@hotmail.com
# Date   : 06/05/2019

# The AVR device you compile for
DEVICE     = atmega328p
# Target AVR clock rate in Hertz
CLOCK      = 16000000
# Options to avrdude which define the hardware you use for uploading
# to the AVR and the interface where this hardware is connected
PROGRAMMER = -c arduino -P /dev/ttyACM0 -b 115200
# The object files created from your source files.
# This list is usually the same as the list of source files with suffix ".o"
OBJECTS    = led.o 
# Parameters for avrdude to flash the fuses appropriately
FUSES      = -U lfuse:w:0xff:m -U hfuse:w:0xde:m -U efuse:w:0xfd:m

# Tune the lines below only if you know what you are doing

AVRDUDE    = avrdude $(PROGRAMMER) -p $(DEVICE)
COMPILE    = avr-gcc -Wall -Os -DF_CPU=$(CLOCK) -mmcu=$(DEVICE)

# Symbolic targets:

all: led.hex

.c.o: 
    $(COMPILE) -c $< -o $@

.S.o:
    $(COMPILE) -x assembler-with-cpp -c $< -o $@

# "-x assembler-with-cpp" should not be necessary since this is the default
# file type for the .S (with capital S) extension. However, upper case characters are not always preserved on Windows
# To ensure WinAVR compatibility define the file type manually.

.c.s:
    $(COMPILE) -S $< -o $@

flash: all
    $(AVRDUDE) -U flash:w:led.hex:i

fuse:
    $(AVRDUDE) $(FUSES)

install: flash fuse

# if you use a bootloader, change the command below appropriately:

load: all
    bootloadHID led.hex

clean: 
    rm -f led.hex led.elf $(OBJECTS)

# file targets:

led.elf: $(OBJECTS)
    $(COMPILE) -o led.elf $(OBJECTS)

led.hex: led.elf
    rm -f led.hex
    avr-objcopy -j .text -j .data -O ihex led.elf led.hex

# If you have an EEPROM section, you must also create a hex file for the
# EEPROM and add it to the "flash" target.

# Targets for code debugging and analysis:

disasm: led.elf
    avr-objdump -d led.elf

cpp:
    $(COMPILE) -E led.c
    
