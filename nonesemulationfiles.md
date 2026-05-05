# Nonesemulationfiles

> Source: https://problemkaputt.de/everynes.htm
> Section: Nonesemulationfiles

No$nes Emulation Files 
**SLOT Folder**

Default location for Game ROM-Images is "SLOT" folder (in same directory as
"no$nes.exe").

ZIPped ROM-Images can be loaded if PKUNZIP.EXE is installed (ie. it must be
somewhere in your "PATH") (if you don't know what that means, put it into a
folder like C:\WINDOWS\COMMAND or so).

**BIOS Folder**

The "BIOS" folder (in same directory as "no$nes.exe") should contain following
files:

```
DISKSYS.ROM   8Kbytes  Famicom Disk System (FDS) BIOS
PC10BIOS.ROM  16Kbytes Playchoice 10 (PC10) BIOS    (IC "8T")
PC10CHAR.ROM  24Kbytes Playchoice 10 (PC10) Charset (ICs "8P+8M+8K")
```

**Playchoice 10 Games**

PC10 games require the BIOS files (see above), and completely dumped ROMs. As
of 2012, most or all existing PC10 dumps are incomplete: They contain only the
NES PRG/CHR-ROMs, and the Z80 INST-ROM, but do miss the Z80 PROM (which is
absolutely required to use/decrypt the INST-ROMs). A tool for adding the
missing PROM data to existing ROM-images can be found here:
http://problemkaputt.de/pc10make.zip
