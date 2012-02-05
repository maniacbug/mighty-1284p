# Mighty 1284P: Platform files for Arduino to run on ATmega1284P

## What is it?

Everything you need to run Arduino on ATmega1284P.

## Current state

Everything is here and has been initially tested.  It should work fine.  There is more detailed testing which must be completed before calling it completely 'done' though.

The platform now includes optiboot.  This bootloader is better in every way than the previous version, so it is recommended in all cases.  The prior one is included for reference.

## Installation

1. Download the [ZIP File](https://github.com/maniacbug/mighty-1284p/zipball/master)
2. Unzip it a folder called 'hardware' off your sketches directory, e.g. /Users/maniacbug/Source/Arduino/hardware/mighty-1284p
3. Restart the IDE
4. Select Tools > Board > Mighty 1284p 16MHz using Optiboot

## Requirements

* Works only on Arduino >= 1.0
* Cannot be burned using [USBtinyISP](http://www.ladyada.net/make/usbtinyisp/).  That programmer cannot flash to chips with >64k flash size.

## See also

http://maniacbug.wordpress.com/2011/11/27/arduino-on-atmega1284p-4/
