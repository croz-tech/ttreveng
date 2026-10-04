# Reversing the Acorn Teletext Adapter Hardware and Software

## Introduction

I've had a teletext adapter for a while (bought cheap on eBay some years ago).  At the time I started repairing the hardware and then shelved the project.  Now was time to resurrect this and get it working.  Unfortunately the hardware was in a pretty bad shape.  When I got this both PSU fuses (+5V and +12V) were blown and it looked like the +40V tuning supply wire had been floating around inside the unit and randomly frying ICs.

Powering up the adapter alone with 5V from a bench supply, there was clearly a short to ground.  Limiting the current to 1A and probing around between VCC and GND on all of the 5V logic I was able to find and replace three clearly shorted ICs, including both 2114 SRAMs.  The fun didn't stop there though.  As troubleshooting proceeded I found various dead logic ICs, replacing a total of six ICs.

Unfortunately when connecting to the beeb and running the archived `TFS103` ROM, I found the machine was hanging with after the `TFS` message and not relinquishing control of the onboard RAM to fill the next teletext pages.

After a lot of scope probing and hard scratching, without much knowledge of what either the teletext adapter circuit or ROM was doing, and being somewhat inspired by [Rob Smallshire's TNMOC talk](https://youtu.be/ubDdlEGjWQc?si=Byj29Ef7eOVyyhH_); I turned to AI for some assistance, initially asking it to study the circuit and disassemble the ROM.

Without further prompt it discovered that the `ATS103` ROM I was using contained a bad byte and would hang (I hadn't even told it this was what I was seeing).  It patched the ROM for me, and suddenly things sprang to life.  I was seeing some `bad data` messages at the bottom of the screen, so asked for further help, which was explained away.

As it happened, when looking on [stardot I found a previous post](https://stardot.org.uk/forums/viewtopic.php?p=455936&hilit=Teletext+rom+version#p455936) where someone else had discovered the same and posted an updated `TELEROM` file (here referred to as `TELEROMU.ROM`).

## Disassembly & Reverse Engineering
However, I was so impressed with how quickly the AI (claude code in this case) had come to a conclusion about the ROM file, I asked it to:
- disassemble the correct file `TELEROMU.ROM`
- disassemble the update `ATS250.ROM` file (this doesn't have the CRC issue, so using this now)
- create a detailed circuit description to aid any future repairs

This is all documented in this repository.  The disassembled files are in the [/disassembly](https://github.com/croz-tech/ttreveng/tree/main/disassembly) folder, and an overview, hardware description and the dissasemblies are in a single [TFS-ATS-reference.pdf](https://github.com/croz-tech/ttreveng/blob/main/TFS-ATS-reference.pdf) file.  I hope folks find this helpful.

The root folder also contains the original ROMs, Uses guides for both TFS and ATS, the schematic (credit ZXGuesser for redrawing the poor scan into a legible format), and some useful datasheets which are now harder to find.

Please advise if any of the information in here infringes any valid copyright.

## Some Images
Board top side

<img src="/images/top%20of%20the%20board%20(for%20ref).jpg" width="400">

Board bottom side (bodge wires are from manufacture)

<img src="/images/existing%20bodge%20wires%20(for%20ref).jpg" width="400">

After issues were resolved/with added input for raspberry Pi CVBS source

<img src="/images/finished%2C%20with%20CVBS%20input%20for%20RPi.jpg" width="400">

Resolving problematic ICs with the thermal camera

<img src="/images/some%20hot%20ICs.JPG" width="400">

Everything working

<img src="/images/working%20teletext.jpg" width="600">

and what a screen of working teletext looks like (pixel perfect from the RGBtoHDMI)

<img src="/images/capture3.png">
