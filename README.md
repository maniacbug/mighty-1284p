# Mighty 1284P: Platform files for Arduino to run on ATmega1284P

## What is it?

Everything you need to run Arduino on ATmega1284P.

## Current state

This is a fork of Maniacbug's work, which seems to have been abandoned.  It uses upstream Arduino files as much as possible instead of (out of date) copies.  It has been tested and is considered complete.  Please report any bugs you may find.

## Installation

Recommended method: on your Debian system, install the package from the repository.  Alternatively:

1. Download the [ZIP File](https://github.com/wijnen/mighty-1284p/zipball/master)
2. Unzip it a folder called 'hardware' in your sketchbook directory, e.g. ~/Arduino/hardware/mighty-1284p
3. Restart the IDE
4. Select Tools > Board > Mighty 1284p 16MHz using Optiboot
5. To burn the bootloader, follow the Arduino [Bootloader](http://arduino.cc/en/Hacking/Bootloader) instructions.

## Requirements

* Works only on Arduino >= 1.0
* Cannot be burned using [USBtinyISP](http://www.ladyada.net/make/usbtinyisp/).  That programmer cannot flash to chips with >64k flash size.

## See also

http://maniacbug.wordpress.com/2011/11/27/arduino-on-atmega1284p-4/

## Supported Boards

* 'Mighty 1284p using Optiboot'.
* 'Original Mighty 1284p'.  Uses a different booloader; if you have a board with this, burning optiboot is recommended.
