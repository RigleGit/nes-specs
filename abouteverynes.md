# Abouteverynes

> Source: https://problemkaputt.de/everynes.htm
> Section: Abouteverynes

About Everynes 
**Everynes**

Everything about NES and Famicom.

Nocash Technical Specifications written 2004 by Martin Korth.

Everynes Text and Html and Debugger versions and updates available at:

```
http://problemkaputt.de/nes.htm
```

I've originally written Everynes when collecting and sorting-out relevant info
for making the no$nes emulator/debugger. I've included a copy of the resulting
document in the debuggers help text, and also released raw txt/htm versions,
which may be eventually of some use to NES/Famicom programmers.

**Help welcome**

Please let me know if you come across anything that is incomplete, incorrect,
or unclear. A couple of details (marked by question marks) are definetly
unclear to me - additional info would be very welcome!

My email address hides in no$nes.exe about box (for anti-spam reasons).

**Thanks & Credits**

Most of the Everynes document is based on information from many other documents
found at nesdev.parodius.com - hoping that nobody gets angry about picking info
from his/her docs - I'd like to send many thanks to the authors of that great
documents, and to all people whom have contributed information to those docs,
complete list as far as known to me - many thangs to:

**(Feedback)**

Thanks to Joe Lennox, and #nesdev on EFnet for Everynes corrections.

**MAPPERS.NFO**

Comprehensive NES Mapper Document v0.80 by \Firebug\

Thanks to FanWen, Y0SHi, D, Jim Geffre, Goroh, Paul Robson, Mark Knibbs.

**2A03TECH.TXT**

2A03 technical reference by Brad Taylor

Thanks to Matthew Conte, Kentaro Ishihara, Goroh, Memblers, FluBBa, Izumi,

Chibi-Tech, Quietust, SnowBro, Bananmos, Kevin Horton, and many others for

their time and help on and off the NESdev mailing list, and the Membled

Messageboards.

**2C02TECH.TXT**

NTSC 2C02 technical reference by Brad Taylor

Thanks to the NES community. http://nesdev.parodius.com.

Special thanks to Neal Tew for scrolling information.

**FDSTECH.TXT**

Famicom Disk System Technical Reference by Brad Taylor. Special thanks to Nori,
Goroh, Ki, Sgt. Bowhack and "D".

**NESTECH2.TXT**

Nintendo Entertainment System Documentation, Version: 2.00

Alex Krasivsky

Andrew Davie

Avatar Z

Barubary

Bluefoot

CiXeL

Chi-Wen Yang

Chris Hickman

D

Dan Boris

David de Regt

Donald Moore

Fredrik Olsson

Icer Addis

Jon Merkel

Kevin Horton

Loopy

Marat Fayzullin

Mark Knibbs

Martin Nielsen

Matt Conte

Matthew Richey

Memblers

MiKael Iushin

Mike Perry

Morgan Johansson

Neill Corlett

Pat Mccomack

Patrik Alexandersson

Paul Robson

Ryan Auge

Stumble

Tennessee Carmel-Veilleux

Thomas Steen

Tony Young

Vince Indriolo

\FireBug\

**FFPA.TXT**

Famicom Four-Player Adapters Technical Document by Richard Hoelscher

**NES4PLAY.TXT**

NES 4player-adapter documentation by Fredrik Olsson

Special thanks to:

Juan Antonio Gomez Galvarez

Yoshi

Marat Fayzulin

Morgan Johansson

**Pin-Outs**

drk421

Siudym'2001

**6205BUGS.TXT**

Ivo van Poorten

**NLOCKOUT.TXT**

by Mark (Knipps?)

**VSDOC.TXT**

VS Unisystem information version 1.0, by Fx3

**PC10DOC.TXT**

Nintendo Playchoice 10 Hardware Description by Oliver Achten

**NESGG.TXT**

NES Game Genie Code Format DOC v0.71 by Benzene of Digital Emutations

Special thanks to Sardu, Opcode, Deuce, DrSplat, KingPin

**NESFQ.HTM**

NES Tech FAQ by Chris Covell

**NES.HTM**

Nintendo Entertainment System Architecture by Marat Fayzullin

```
Pascal Felber     Patrick Lesaard      Tink
Goroh             Pan of Anthrox       Bas Vijfwinkel
Kawasedo          Paul Robson
Marcel de Kogel   Serge Skorobogatov
Alex Krasivsky    John Stiles
```

**KEYBOARD.TXT**

Reverse Engineering the Keyboard of Family Computer

by goroh, english translation by Ki

**LIGHTGUN.TXT**

Family Computer Gun

by goroh, english translation by Ki

**POWERPAD.TXT**

Power Pad information Version: 1.2 (03/12/00) by Tennessee Carmel-Veilleux

Thanks to Jeremy D. Chadwick, Kevin Horton

**CPU**

Project64, Graham

**The weird and wonderful CIC**

Segher's rev-engineered instruction set for the CIC CPU.

Index

Contents

No$nes Controls

XED Editor

XED About

XED Hotkeys

XED Assembler/Debugger Interface

XED Commandline based standalone version

No$nes Emulation Files

Tech Data

Memory Maps

I/O Map

Picture Processing Unit (PPU)

PPU Reset

PPU Control and Status Registers

PPU SPR-RAM Access Registers

PPU VRAM Access Registers

PPU Scrolling

PPU Tile Memory

PPU Background

PPU Sprites

PPU Palettes

PPU Dimensions & Timings

PPU 2C02 Timings

3D Glasses

Audio Processing Unit (APU)

APU Channel 1-4 Register 0 (Volume/Decay)

APU Channel 1-4 Register 1 (Sweep)

APU Channel 1-4 Register 2 (Frequency)

APU Channel 1-4 Register 3 (Length)

APU Channel 5 - DMC Sound

APU Control and Status Registers

APU 4-bit DAC

APU Various

APU DMC-DMA Glitch

APU External Sound Channels

Controllers

Controllers - I/O Ports

Controllers - Pin-Outs

Controllers - Summary of Controller Types

Controllers - Summary of Controller Signals

Controllers - Detection

Controllers - Joypads

Controllers - Four-Player Adaptors

Controllers - Lightguns (Zapper)

Controllers - Paddles

Controllers - Push Buttons

Controllers - Typewriter Keyboards

Controllers - Piano Keyboards

Controllers - Piano - Miracle Piano Controller Port

Controllers - Piano - Miracle Piano MIDI Commands

Controllers - Piano - Miracle Piano Instruments

Controllers - Piano - Miracle Pinouts and Component List

Controllers - Keypads

Controllers - Mats

Controllers - Inflatable Controllers

Controllers - RacerMate Bicycle Training System

Controllers - Tablets

Controllers - Trackball and Mouse

Controllers - Power Glove

Controllers - Power Glove Transmission Protocol (RX/TX)

Controllers - Power Glove TX Packets (Configuration Opcodes)

Controllers - Power Glove RX Packets (Position/Sensor Data)

Controllers - Power Glove Games and Compatibilty Modes

Controllers - Power Glove Drawings

Controllers - Power Glove Pinouts

Controllers - UForce

Controllers - UForce I/O

Controllers - UForce Drawings

Controllers - UForce Games and Game Switches

Controllers - Barcode Readers

Controllers - Barcode Battler (barcode reader)

Controllers - Barcode Battler Transmission I/O

Controllers - Barcode Battler Drawings

Controllers - Barcode Formats

Controllers - Pachinko

Controllers - Microphones

Controllers - Reset Button

Controllers - Arcade Machines

Storage Data Recorder

Storage Turbo File

Storage Battle Box

Storage Battle Box I/O Access

Storage Battle Box Filesystem

Hori Game Repeater

Headphones

R.O.B. (Robotic Operating Buddy)

R.O.B. Technical Drawings

Cartridges and Mappers

Cartridge Overview

Cartridge ROM-Image File Formats

Cartridge IRQ Counters

Cartridge Bus Conflicts

Cartridge Cicurity Chip (CIC) (Lockout Chip)

Cartridge CIC Pseudo Code

Cartridge CIC Instruction Set

Cartridge CIC Notes

Cartridge CIC Versions

Cartridge CIC Pinouts

Cartridge CIC Tengen Clone

Cartridge Cheat Devices

Cartridge Pin-Outs

Cartridge Shell Dimensions

Mapper 0: NROM - No Mapper (or unknown mapper)

Mapper 1: MMC1 - PRG/32K/16K, VROM/8K/4K, NT

Mapper 2: UNROM - PRG/16K

Mapper 3: CNROM - VROM/8K

Mapper 4: MMC3 - PRG/8K, VROM/2K/1K, NT, SRAM, IRQ

Mapper 5: MMC5 - BANKING, IRQ, SOUND, VIDEO, MULTIPLY, etc.

Mapper 5: MMC5 - I/O Map

Mapper 5: MMC5 - CPU Memory Control

Mapper 5: MMC5 - Video Name Table

Mapper 5: MMC5 - Video Pattern Table

Mapper 5: MMC5 - Video Split and IRQ and Multiply Unit

Mapper 5: MMC5 - Video EXRAM

Mapper 5: MMC5 - Sound Control

Mapper 6,8,12,17: Front Far East (FFE) Configuration, IRQs, Patches

Mapper 6: FFE F4xxx - PRG/16K, VROM/8K, NT, IRQ

Mapper ?: FFE F4xxx - PRG/16K, VROM/8K, NT, IRQ

Mapper 7: AOROM - PRG/32K, Name Table Select

Mapper 8: FFE F3xxx - PRG/32K, VROM/8K, NT, IRQ

Mapper 9: MMC2 - PRG/24K/8K, VROM/4K, NT, LATCH

Mapper 10: MMC4 - PRG/16K, VROM/4K, NT, LATCH

Mapper 11: Color Dreams - PRG/32K, VROM/8K

Mapper 12: FFE F6xxx - Not specified, NT, IRQ

Mapper 13: CPROM - 16K VRAM

Mapper 15: X-in-1 - PRG/32K/16K, NT

Mapper 16: Bandai - PRG/16K, VROM/1K, IRQ, EPROM

Mapper 17: FFE F8xxx - PRG/8K, VROM/1K, NT, IRQ

Mapper 18: Jaleco SS8806 - PRG/8K, VROM/1K, NT, IRQ, EXT

Mapper 19: Namcot 106 - PRG/8K, VROM/1K/VRAM, IRQ, SOUND

Mapper 20: Disk System - PRG RAM, BIOS, DISK, IRQ, SOUND

Mapper 21: Konami VRC4A/VRC4C - PRG/8K, VROM/1K, NT, IRQ

Mapper 22: Konami VRC2A - PRG/8K, VROM/1K, NT

Mapper 23: Konami VRC2B/VRC4E - PRG/8K, VROM/1K, NT, (IRQ)

Mapper 24: Konami VRC6A - PRG/16K/8K, VROM/1K, NT, IRQ, SOUND

Mapper 25: Konami VRC4B/VRC4D - PRG/8K, VROM/1K, NT, IRQ

Mapper 26: Konami VRC6B - PRG/16K/8K, VROM/1K, NT, IRQ, SOUND

Mapper 21-26,73,75,85: Konami VRC Mappers

Mapper 28: Action 53 homebrew X-in-1

Mapper 32: Irem G-101 - PRG/8K, VROM/1K, NT

Mapper 33: Taito TC0190/TC0350 - PRG/8K, VROM/1K/2K, NT, IRQ

Mapper 34: Nina-1 - PRG/32K, VROM/4K

Mapper 40: FDS-Port - Lost Levels

Mapper 41: Caltron 6-in-1

Mapper 42: FDS-Port - Mario Baby

Mapper 43: X-in-1

Mapper 44: 7-in-1 MMC3 Port A001h

Mapper 45: X-in-1 MMC3 Port 6000hx4

Mapper 46: 15-in-1 Color Dreams

Mapper 47: 2-in-1 MMC3 Port 6000h

Mapper 48: Taito TC190V

Mapper 49: 4-in-1 MMC3 Port 6xxxh

Mapper 50: FDS-Port - Alt. Levels

Mapper 51: 11-in-1

Mapper 52: 7-in-1 MMC3 Port 6800h with SRAM

Mapper 56: Pirate SMB3

Mapper 57: 6-in-1

Mapper 58: X-in-1

Mapper 61: 20-in-1

Mapper 62: X-in-1

Mapper 64: Tengen RAMBO-1 - PRG/8K, VROM/2K/1K, NT, IRQ

Mapper 65: Irem H-3001 - PRG/8K, VROM/1K, NT, IRQ

Mapper 66: GNROM - PRG/32K, VROM/8K

Mapper 67: Sunsoft3 - PRG/16K, VROM/2K, IRQ

Mapper 68: Sunsoft4 - PRG/16K, VROM/2K, NT-VROM

Mapper 69: Sunsoft5 FME-7 - PRG/8K, VROM/1K, NT ctrl, SRAM, IRQ

Mapper 70: Bandai - PRG/16K, VROM/8K, NT

Mapper 71: Camerica - PRG/16K

Mapper 72: Jaleco Early Mapper 0 - PRG-LO, VROM/8K

Mapper 73: Konami VRC3 - PRG/16K, IRQ

Mapper 74: Whatever MMC3-style

Mapper 75: Jaleco SS8805/Konami VRC1 - PRG/8K, VROM/4K, NT

Mapper 76: Namco 109 - PRG/8K, VROM/2K

Mapper 77: Irem - PRG/32K, VROM/2K, VRAM 6K+2K

Mapper 78: Irem 74HC161/32 - PRG/16K, VROM/8K

Mapper 79: AVE Nina-3 - VROM/8K

Mapper 80: Taito X-005 - PRG/8K, VROM/2K/1K, NT

Mapper 81: AVE Nina-6

Mapper 82: Taito X1-17 - PRG/8K, VROM/2K/1K

Mapper 83: Cony

Mapper 84: Whatever

Mapper 85: Konami VRC7A/B - PRG/16K/8K, VROM/1K, NT, IRQ, SOUND

Mapper 86: Jaleco Early Mapper 2 - PRG/32K, VROM/8K

Mapper 87: Jaleco/Konami 16K VROM - VROM/8K

Mapper 88: Namco 118

Mapper 89: Sunsoft Early - PRG/16K, VROM/8K

Mapper 90: Pirate MMC5-style

Mapper 91: HK-SF3 - PRG/8K, VROM/2K, IRQ

Mapper 92: Jaleco Early Mapper 1 - PRG-HI, VROM/8K

Mapper 93: 74161/32 - PRG/16K

Mapper 94: 74161/32 - PRG/16K

Mapper 95: Namcot MMC3-Style

Mapper 96: 74161/32 - PRG/32K, CHR/16K/4K, LATCH

Mapper 97: Irem - PRG HI

Mapper 99: VS Unisystem Port 4016h - VROM/8K, (PRG/8K)

Mapper 100: Whatever

Mapper 105: X-in-1 MMC1

Mapper 112: Asder - PRG/8K, VROM/2K/1K

Mapper 113: Sachen/Hacker/Nina

Mapper 114: Super Games

Mapper 115: MMC3 Cart Saint

Mapper 116: Whatever

Mapper 117: Future

Mapper 118: MMC3 TLSROM - PRG/8K, VROM/2K/1K, Banked-NT, SRAM, IRQ

Mapper 119: MMC3 TQROM - PRG/8K, VROM/VRAM/2K/1K, NT, SRAM, IRQ

Mapper 122: Whatever

Mapper 133: Sachen

Mapper 151: VS Unisystem VRC1 or MMC3 Daughterboards

Mapper 152: Whatever

Mapper 160: Same as Mapper 90

Mapper 161: Same as Mapper 1

Mapper 168: RacerMate PRG/16K, VRAM/4K, IRQ

Mapper 180: Nihon Bussan - PRG HI

Mapper 182: Same as Mapper 114

Mapper 184: Sunsoft - VROM/4K

Mapper 185: VROM-disable

Mapper 188: UNROM-reversed

Mapper 189: MMC3 Variant

Mapper 218: Nocash Single-Chip

Mapper 222: Dragon Ninja

Mapper 225: X-in-1

Mapper 226: X-in-1

Mapper 227: X-in-1

Mapper 228: X-in-1 Homebrewn

Mapper 229: 31-in-1

Mapper 230: X-in-1 plus Contra

Mapper 231: 20-in-1

Mapper 232: 4-in-1 Quattro Camerica

Mapper 233: X-in-1 plus Reset

Mapper 234: Maxi-15

Mapper 240: C&E/Supertone - PRG/32K, VROM/8K

Mapper 241: X-in-1 Education

Mapper 242: Waixing - PRG/32K, NT

Mapper 243: Sachen Poker - PRG/32K, VROM/8K

Mapper 244: C&E - PRG/32K, VROM/8K

Mapper 246: C&E - PRG/8K, VROM/2K, SRAM

Mapper 255: X-in-1 - (Same as Mapper 225)

Famicom Disk System (FDS)

FDS Memory and I/O Maps

FDS I/O Ports - Timer

FDS I/O Ports - Disk

FDS I/O Ports - Sound

FDS BIOS Disk Format

FDS BIOS Disk Functions

FDS BIOS Disk Errors

FDS BIOS Data Areas in WRAM

FDS Disk Drive Operation

VS System

VS System Controllers

VS System Games

VS System PPUs and Palettes

VS System Protections

Nintendo Playchoice 10

PC10 Memory Map and I/O Ports

PC10 Video Circuit

PC10 Title/Instructions (INST ROM)

PC10 NES-Side

PC10 Games and Cartridge PCBs

PC10 Cabinet and BIOS Versions

PC10 Pin-Outs

Z80 CPU Specifications

Z80 Register Set

Z80 Flags

Z80 Instruction Format

Z80 Load Commands

Z80 Arithmetic/Logical Commands

Z80 Rotate/Shift and Singlebit Operations

Z80 Jumpcommands & Interrupts

Z80 I/O Commands

Z80 Interrupts

Z80 Meaningless and Duplicated Opcodes

Z80 Garbage in Flag Register

Z80 Compatibility

Z80 Pin-Outs

Z80 Local Usage

FamicomBox

FamicomBox Memory and I/O Maps

FamicomBox I/O Ports

FamicomBox Misc

FamicomBox Cartridges

FamicomBox ROM Header (at FFE0h)

FamicomBox Pinouts

Modems

Unpredictable Things

CPU 65XX Microprocessor

CPU Registers and Flags

CPU Memory Addressing

CPU Memory and Register Transfers

CPU Arithmetic/Logical Operations

CPU Rotate and Shift Instructions

CPU Jump and Control Instructions

CPU Illegal Opcodes

CPU Assembler Directives/Syntax

CPU Glitches

CPU The 65XX Family

CPU Local Usage

Hardware Pin-Outs

Chipset Pin-Outs

NES Expansion Port

Nocash SRAM Circuit

About Everynes

[extracted from no$nes v1.2 documentation]
