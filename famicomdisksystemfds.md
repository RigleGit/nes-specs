# Famicomdisksystemfds

> Source: https://problemkaputt.de/everynes.htm
> Section: Famicomdisksystemfds

Famicom Disk System (FDS) 
Famicom Disk System (FDS) is a Famicom extension unit which was produced by
Nintendo and only sold in Asian countries. It consists of a disk drive
accepting 2.5" or 3" (?) floppies, 32K of RAM to load programs into, 8K of
VRAM, and some other hardware described below.

FDS Memory and I/O Maps

FDS I/O Ports - Timer

FDS I/O Ports - Disk

FDS I/O Ports - Sound

FDS BIOS Disk Format

FDS BIOS Disk Functions

FDS BIOS Disk Errors

FDS BIOS Data Areas in WRAM

FDS Disk Drive Operation

FDS I/O Ports - Timer

Interrupt Timer intended to produce mid-screen scanline interrupts.

**4020h - Timer IRQ Counter Reload value LSB (W)**

**4021h - Timer IRQ Counter Reload value MSB (W)**

```
Reload value loaded to actual 16bit counter register on write to 4022h,
and on counter underflow. Counter is decremented once per CPU clock cycle.
```

**4022h - Timer IRQ Enable/Disable (W)**

```
Bit1  Enable  (0=Stop/Acknowledge? Timer IRQ, 1=Start/Enable Timer IRQ)
```

Note: Timer IRQ Flag is found in Bit0 of Port 4030h (Disk Status Register 0);
reading that status register does (also?) acknowledge IRQs.

The IRQ Vector is controlled by the BIOS via [0101h] and [DFFEh],

FDS BIOS Data Areas in WRAM

FDS I/O Ports - Sound

**4040h..407Fh - Wave RAM - 64 x 6bit sample data (Read/Write)**

Writes to these registers are ignored unless Write Mode is turned on (see
register 4089h).

**4089h - Wave RAM Control (Write Only)**

```
Bit7    Wave Write Mode (1=Stop Sound output & Allow to write to Wave RAM)
Bit6-2  Not used
Bit1-0  Master Volume   (0-3 = 100%,66%,50%,40% = 30/30,20/30,15/30,12/30)
```

**4082h - Wave RAM Sample Rate LSB (Write Only)**

```
Bit7-0  Lower 8 bits of the main unit's frequency (upper 4 bits in 4083h)
```

**4083h - Wave RAM Sample Rate MSB and Control (Write Only)**

```
Bit7    Main Unit disable  (0=Enable, 1=Disable Sound Output)
Bit6    Envelope disable   (0=Normal, 1=Disable Volume/Sweep Envelopes)
Bit5-4  Not used
Bit3-0  Upper 4 bits of the main unit's frequency
```

Main Unit / Sample Rate: (per entry of the 64-entry wave ram)

```
F = 1.79MHz * (Freq + Mod) / 65536
Mod = Frequency change based on the Modulation unit
```

If the 12bit frequency is zero, the Main unit is disabled (channel silent).

**408Ah - Envelope Base Frequency (Write Only)**

```
Bit7-0  Envelope Base Frequency, Fbase=1.79MHz/8/N
```

Fbase used by 4080h and 4084h. Volume/Sweep Envelope are disabled if N=0.

**4080h - Volume Envelope (Write Only)**

```
Bit7    Volume Envelope Mode      (0=Volume Envelope, 1=Fixed Volume)
Bit6    Volume Envelope Direction (When enabled / at specified rate)
0=Decrease Volume by 1 (only if Volume>00h)
1=Increase Volume by 1 (only if Volume193 then temp -= 258;  // not a typo... for some reason the wraps
if temp
FDS BIOS Disk Functions

**Disk Boot**

On power-up, the FDS does call the Load Files function to load the boot files.
The DiskID is FFh-filled (wildcards), except the Side Number and Disk Number
entries which must be both 00h on boot-able disks. LoadList is empty,
indicating to load all boot files, ie. all files with File IDs less or equal
than the Disk Header's Boot ID value.

There must be at least two boot files on the disk, one containing the program
with entrypoint (16bit pointer, which must be loaded to DFFCh), the other file
containing the Nintendo License string (E0h bytes, which must be loaded to PPU
2800h, and which is verified against a copy in BIOS at ED37h).

**DiskID**

Used by most BIOS functions to ensure that the correct disk/side is inserted.
DiskID consists of 10 bytes which are compared against Disk Header Block
[0Fh..18h]. Each DiskID byte may be set to FFh, which is used as wildcard,
comparision always passes okay for that bytes. If the comparision does not
match then error codes 04h..10h are returned, indicating which entry didn't
match.

**E1F8h - Load Files**

The function scans <all> files on disk, respectively, the execution time
is the same, no matter how many/how large files are loaded. On the contrary,
multiple calls to LoadFiles would be unneccessarily slow.

```
RETaddr:        pointer to DiskID
RETaddr+2:      pointer to LoadList
A on return:    error code
Y on return:    count of files actually found
```

LoadList is a list of up to 20 File IDs, if the list contains less than 20 IDs
then it must be terminated by FFh. If LoadList is empty (FFh in the first byte)
then the boot files are loaded, that are all files with File IDs less or equal
than the Disk Header's Boot ID value.

**E32Ah - Get Disk Information**

```
RETaddr:        pointer to DiskInfo
A on return:    error code
```

The DiskInfo pointer should point to a free memory location, which will receive
the following data:

```
0Ah bytes   Disk Header Block [0Fh..18h], manufacturer, disk name, etc.
1   byte    File Number Block [01h], number of files on disk (N)
N*9 bytes   File Header Block [02h..0Ah], File ID and Filename, for each file
2   bytes   Disk Size (MSB,LSB)
```

Disk size is equal to the sum of each file's size entry, plus an extra 261 per
file.

**E237h - Append File**

Sets A=FFh (append after last file), ie. same as A=FileCount, then continues at
E239h (Write File).

**E239h - Write File**

Register A specifies how many old files are to be kept preserved on disk, these
files are read/skipped, and the new file is then written to disk after those
files, and the disks FileCount is set to A+1, making the new file to be the
last file on disk, any further files are deleted/hidden.

In a second cycle, the written data is verified, if the verification fails
(error 26h), then the file count is decremented, ie. the new file is deleted.

```
RETaddr:        pointer to DiskID
RETaddr+2:      pointer to FileInfo
A on call:      File Number  (00h=First) (FFh=Append after last file)
A on return:    error code
```

FileInfo occupies 17 bytes, the first 14 bytes contain File Header entries
[02h..0Fh], ie. File ID, Filename, Load Address, Filesize, and Load Area.

The last 3 bytes contain the Source Address and Source Area (which may or may
not be same as Load Address and Load Area).

**E2BBh - Adjust File count**

```
RETaddr:        pointer to DiskID
A on call:      number to reduce current file count by
A on return:    error code
Special error:  #$31 if A is less than the disk's file count
```

Reads in disk's file count, decrements it by A, then writes the new value back.

**E2B7h - Check File count**

```
RETaddr:        pointer to DiskID
A on call:      number to set file count to
A on return:    error code
Special error:  #$31 if A is less than the disk's file count
```

Reads in disk's file count, compares it to A, then sets the disk's file count
to A.

**E305h - Set File count (alt. 1)**

```
RETaddr:        pointer to DiskID
A on call:      number to set file count to
A on return:    error code
```

Sets the disk's file count to A.

**E301h - Set File count (alt. 2)**

```
RETaddr:        pointer to DiskID
A on call:      number to set file count to minus 1
A on return:    error code
```

Sets the disk's file count to A+1.

Don't expect disk calls to return quick; it may take several seconds to
complete. The ROM BIOS always uses disk IRQ's to transfer data between the
disk, so programs must surrender IRQ control to the ROM BIOS during these disk
calls. The value at [$0101] however, is preserved on entry, and restored on
exit.

**WRAM Target Addresses**

Target Addresses should be in range 0200h-07FFh or 6000h-DFFFh. The BIOS
rejects (silently ignores) most attempts to load data to 0000h-01FFh. In
particular, it rejects Target Addresses at 0-1FFh (including mirrors at
800h,1000h,1800h), it also rejects wraps from FFFFh to 0000h.

However, it does not reject wraps to mirrors (eg. from 7FFh to 800h), clever
use of this feature might allow to modify values on stack, and to bypass the
Boot License.

Furthermore, target address 2000h can be used to enable NMIs during loading,
the games Bislot, Bisyosya, and Bishojo Control are using this trick to abort
the boot process and to start the game - without Boot License - via NMI vector
at [DFFAh].

**VRAM Source/Target Addresses**

Mind that the screen should be disabled when loading/writing VRAM data, Port
2001h/Bit3-4 should be zero, otherwise VRAM could be accessed only in VBlank.
Mind that physical content of VRAM addresses 2400h-2BFFh changes depending on
current mirroring. The BIOS re-initializes parts of VRAM after loading the boot
files on power up, overwriting any boot-files loaded to that areas.

**Boot License String**

32x7 characters in VRAM 2800h-28DFh (and copy in BIOS at ED37h)

```
"           NINTENDO r           "
"       FAMILY COMPUTER TM       "
"                                "
"  THIS PRODUCT IS MANUFACTURED  "
"  AND SOLD BY NINTENDO CO;LDT.  "
"  OR BY OTHER COMPANY UNDER     "
"  LICENSE OF NINTENDO CO;LTD..  "
```

By using Non-ASCII BIOS Tile Numbers: A..Z=0Ah..23h SPC=24h .=26h ;=27h r=28h.

See WRAM Target Addresses above for methods to bypass the Boot License.

FDS BIOS Data Areas in WRAM

The BIOS uses several places in memory, but only some of them are expected to
be maintained by game code.

```
Addr  Size  Expl.
```

**Scratch Area (destroyed by any calls to BIOS disk functions)**

```
0000h  2    first 16bit parameter
0002h  2    second 16bit parameter
0004h  1    previous stack frame
0005h  1    error retry count
0006h  1    file counter
0007h  1    current block type
0008h  1    boot ID code
0009h  1    dummy read flag
000Ah  2    16bit destination address
000Ch  2    16bit transfer length count
000Eh  1    file found counter
```

**Copies of I/O Ports (used to READ content of Write-Only I/O Ports)**

The BIOS does (and the game should) keep these bytes in sync with the ports.

```
00F9h  1    value last written to [$4026]   $FF on reset (disk ext connector)
00FAh  1    value last written to [$4025]   $2E on reset (disk control)
00FBh  1    value last written to [$4016]   0'd on reset (joypad)
00FCh  1    value last written to [$2005]#2 0'd on reset (ppu scrolling)
00FDh  1    value last written to [$2005]#1 0'd on reset (ppu scrolling)
00FEh  1    value last written to [$2001]   $06 on reset (ppu control)
00FFh  1    value last written to [$2000]   $80 on reset (ppu control)
```

**IRQ/NMI/Reset Control**

```
0100h  1    Action on NMI   (set to C0h on reset)
0101h  1    Action on IRQ   (set to 80h on reset)
0102h  2    Action on Reset (AC35h after disk-boot, 5335h after warm-boot)
```

**IRQ/NMI/Reset Vectors**

```
DFF6h  2    Game NMI vector 1, used if [0100h]=01xxxxxxb
DFF8h  2    Game NMI vector 2, used if [0100h]=10xxxxxxb
DFFAh  2    Game NMI vector 3, used if [0100h]=11xxxxxxb
DFFCh  2    Game Reset vector, used if [0102h]=5335h or =AC35h
DFFEh  2    Game IRQ vector,   used if [0101h]=11xxxxxxb
```

If [0100h..0102h] don't match then the IRQ/NMI/Reset is handled internally by
the BIOS, without using (and without changing) the Game vectors.

Address DFFCh contains the initial entrypoint at disk boot, and is also used as
warm-boot vector when pushing the Reset button at a later time.

There may be more structured data areas in the zero page (for example, the BIOS
joypad routines use $F5..$F8 for storing controller reads), but only the listed
ones are used by the disk call subroutines.
