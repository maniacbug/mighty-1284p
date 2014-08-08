# Makefile for ATmegaBOOT
# E.Lins, 18.7.2005
# $Id$
#
# Instructions
#
# To make bootloader .hex file:
# make diecimila
# make lilypad
# make ng
# etc...
#
# To burn bootloader .hex file:
# make diecimila_isp
# make lilypad_isp
# make ng_isp
# etc...

# program name should not be changed...
PROGRAM    = optiboot

# The default behavior is to build using tools that are in the users
# current path variables, but we can also build using an installed
# Arduino user IDE setup, or the Arduino source tree.
# Uncomment this next lines to build within the arduino environment,
# using the arduino-included avrgcc toolset (mac and pc)
# ENV ?= arduino
# ENV ?= arduinodev
# OS ?= macosx
# OS ?= windows

# enter the parameters for the avrdude isp tool
ISPTOOL	   = avrisp 
ISPPORT	   = /dev/ttyUSB0
ISPSPEED   = -b 19200

MCU_TARGET = atmega168
LDSECTIONS  = -Wl,--section-start=.text=0x3e00 -Wl,--section-start=.version=0x3ffe

# Build environments
# Start of some ugly makefile-isms to allow optiboot to be built
# in several different environments.  See the README.TXT file for
# details.

# default
fixpath = $(1)

# the efuse should really be 0xf8; since, however, only the lower
# three bits of that byte are used on the atmega168, avrdude gets
# confused if you specify 1's for the higher bits, see:
# http://tinker.it/now/2007/02/24/the-tale-of-avrdude-atmega168-and-extended-bits-fuses/
#
# similarly, the lock bits should be 0xff instead of 0x3f (to
# unlock the bootloader section) and 0xcf instead of 0x2f (to
# lock it), but since the high two bits of the lock byte are
# unused, avrdude would get confused.

ISPFUSES    = $(AVRDUDE_ROOT)avrdude $(AVRDUDE_CONF) -c $(ISPTOOL) \
              -p $(MCU_TARGET) -P $(ISPPORT) $(ISPSPEED) \
              -e -u -U lock:w:0x3f:m -U efuse:w:0x$(EFUSE):m \
              -U hfuse:w:0x$(HFUSE):m -U lfuse:w:0x$(LFUSE):m
ISPFLASH    = $(AVRDUDE_ROOT)avrdude $(AVRDUDE_CONF) -c $(ISPTOOL) \
              -p $(MCU_TARGET) -P $(ISPPORT) $(ISPSPEED) -V \
              -U flash:w:$(PROGRAM)_$(TARGET).hex -U lock:w:0x2f:m

STK500 = "C:\Program Files\Atmel\AVR Tools\STK500\Stk500.exe"
STK500-1 = $(STK500) -e -d$(MCU_TARGET) -pf -vf -if$(PROGRAM)_$(TARGET).hex \
-lFF -LFF -f$(HFUSE)$(LFUSE) -EF8 -ms -q -cUSB -I200kHz -s -wt
STK500-2 = $(STK500) -d$(MCU_TARGET) -ms -q -lCF -LCF -cUSB -I200kHz -s -wt

OBJ        = $(PROGRAM).o
OPTIMIZE = -Os -fno-inline-small-functions -fno-split-wide-types

DEFS       = 
LIBS       =

CC         = $(GCCROOT)avr-gcc

# Override is only needed by avr-lib build system.

override CFLAGS        = -g -Wall $(OPTIMIZE) -mmcu=$(MCU_TARGET) -DF_CPU=$(AVR_FREQ) $(DEFS)
override LDFLAGS       = $(LDSECTIONS) -Wl,--relax -Wl,--gc-sections -nostartfiles -nostdlib

OBJCOPY        = $(GCCROOT)avr-objcopy
OBJDUMP        = $(call fixpath,$(GCCROOT)avr-objdump)

SIZE           = $(GCCROOT)avr-size

atmega1284: TARGET = atmega1284p
atmega1284: MCU_TARGET = atmega1284p
atmega1284: CFLAGS += '-DLED_START_FLASHES=3' '-DBAUD_RATE=115200' '-DBIGBOOT'
atmega1284: AVR_FREQ = 16000000L
atmega1284: LDSECTIONS  = -Wl,--section-start=.text=0x1fc00
atmega1284: $(PROGRAM)_atmega1284p.hex
atmega1284: $(PROGRAM)_atmega1284p.lst

atmega1284_isp: atmega1284
atmega1284_isp: TARGET = atmega1284p
atmega1284_isp: MCU_TARGET = atmega1284p
# 1024 byte boot
atmega1284_isp: HFUSE = DE
# Low power xtal (16MHz) 16KCK/14CK+65ms
atmega1284_isp: LFUSE = FF
# 2.7V brownout
atmega1284_isp: EFUSE = FD
atmega1284_isp: isp

atmega1284_slow: TARGET = atmega1284p_slow
atmega1284_slow: MCU_TARGET = atmega1284p
atmega1284_slow: CFLAGS += '-DLED_START_FLASHES=3' '-DBAUD_RATE=19200' '-DBIGBOOT'
atmega1284_slow: AVR_FREQ = 16000000L
atmega1284_slow: LDSECTIONS  = -Wl,--section-start=.text=0x1fc00
atmega1284_slow: $(PROGRAM)_atmega1284p_slow.hex
atmega1284_slow: $(PROGRAM)_atmega1284p_slow.lst

atmega1284_slow_isp: atmega1284_slow
atmega1284_slow_isp: TARGET = atmega1284p_slow
atmega1284_slow_isp: MCU_TARGET = atmega1284p
# 1024 byte boot
atmega1284_slow_isp: HFUSE = DE
# Low power xtal (16MHz) 16KCK/14CK+65ms
atmega1284_slow_isp: LFUSE = FF
# 2.7V brownout
atmega1284_slow_isp: EFUSE = FD
atmega1284_slow_isp: isp

#
# Generic build instructions
#
#

isp: $(TARGET)
	$(ISPFUSES)
	$(ISPFLASH)

isp-stk500: $(PROGRAM)_$(TARGET).hex
	$(STK500-1)
	$(STK500-2)

%.elf: $(OBJ)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ $^ $(LIBS)
	$(SIZE) $@

clean:
	rm -rf *.o *.elf *.lst *.map *.sym *.lss *.eep *.srec *.bin *.hex

%.lst: %.elf
	$(OBJDUMP) -h -S $< > $@

%.hex: %.elf
	$(OBJCOPY) -j .text -j .data -j .version --set-section-flags .version=alloc,load -O ihex $< $@

%.srec: %.elf
	$(OBJCOPY) -j .text -j .data -j .version --set-section-flags .version=alloc,load -O srec $< $@

%.bin: %.elf
	$(OBJCOPY) -j .text -j .data -j .version --set-section-flags .version=alloc,load -O binary $< $@
