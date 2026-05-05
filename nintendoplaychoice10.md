# Nintendoplaychoice10

> Source: https://problemkaputt.de/everynes.htm
> Section: Nintendoplaychoice10

Nintendo Playchoice 10 
The Nintendo Playchoice 10 (PC10) arcade cabinets can contain up 10 NES games
(on special cartridges). Aside from the NES-compatible hardware, the thing
contains a Z80 CPU and a custom video circuit for handling game selection,
coin/credits, game title/instructions display and bookkeeping.

PC10 Memory Map and I/O Ports

PC10 Video Circuit

PC10 Title/Instructions (INST ROM)

PC10 NES-Side

PC10 Games and Cartridge PCBs

PC10 Cabinet and BIOS Versions

PC10 Pin-Outs

Z80 CPU Specifications

PC10 Video Circuit

**PC10 VRAM (32x28 character "BG Map")**

VRAM is located at 9000h-97FFh. VRAM is write-only, and can be accessed only
when video output is disabled via Port OUT[00h], to avoid flickering, this
should be usually done only during VBlank (ie. in the Z80 NMI handler).

Each character line occupies 40h bytes (20h words). The words are:

```
Bit0-10  Character number (000h..7FFh) (usually only 000h..3FFh installed)
Bit11-15 Palette number   (00h..1Fh)
```

The upper 2 lines and lower 2 lines are masked for V-Blank period, so actually
used VRAM consists of 28 lines at 9080h..977Fh.

**PC10 Charset (for the Z80 Video Circuit)**

```
'0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ.'!-"",:    abcdefghijkl+ ' ;000-03F
'0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ.'!-"",:mnopq?()/=+ ' ;040-07F
'abcdefghijklABCmEnoHIpqLMNOPrRSTUstuYv!wxyzr' ;080-0BF
'stuvwxyz0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ,-!'()       ' ;0C0-0FF
'0 1 2 3 4 5 6 7 8 9 TIMEInsertCoin                ' ;100-13F
'   Tim       NearUp   FillUpTimeAnd        ' ;140-17F
'' ;180-1BF
'' ;1C0-1FF
'ChannelSelectEnter.ButtonsGameTart                        ' ;200-23F
'                                                                ' ;240-27F
'                                                                ' ;280-2BF
'                                                         ' ;2C0-2FF
'                              ' ;300-33F
'cntrlTM' ;340-37F
'' ;380-3BF
'c1986nintendo ' ;3C0-3FF
```

The 400h characters are stored in three 2764 chips (three 8Kx8 EPROMs): IC
"8P"=CHR0, IC "8M"=CHR1, IC "8K"=CHR2 (each chip containing one color-plane of
the 8-color characters). Optionally, for 800h characters, the mainboard could
be fitted with 27128 chips.

The "" graphics symbols are used by the BIOS (and by instruction pages in
Excitebike, Mario Bros, and Super Mario Bros). There are no japanese
characters.

**PC10 Palette (for the Z80 Video Circuit)**

Below palette table is showing 4:4:4 bit RGB values (all digits are inverted,
0=Brightest, F=Darkest, eg. 0FF=Red, F0F=Green, FF0=Blue).

```
Pal      Pal
00: FFF,000,000,000,000,000,000,000     10: FAF,0BF,000,027,00B,FAF,FA9,F08
01: 94D,FFF,000,00B,024,1FF,AFF,FD8     11: FAF,A5D,303,707,00B,FAF,307,FFF
02: F9F,03F,FAF,000,000,000,FFF,FFF     12: 000,000,000,000,000,000,000,000
03: FFF,000,FFF,9FF,06F,0FF,047,F40     13: 000,000,000,000,000,000,000,000
04: 048,9FF,000,000,000,F95,FFA,FFF     14: 000,048,000,000,000,F9F,72F,FFF
05: F9F,0FF,F0B,00A,000,000,FFF,FFF     15: 000,000,000,000,000,000,000,000
06: F9F,88F,0FF,000,000,000,FFF,FFF     16: 000,000,000,000,000,000,000,000
07: F9F,11F,5FF,000,000,000,FFF,FFF     17: 000,FAF,000,000,000,FCF,039,FFF
08: 000,000,000,000,000,000,000,000     18: F8F,000,00B,000,000,036,FFF,FFF
09: 000,000,000,000,000,000,000,000     19: FAB,2CF,F07,029,111,FFF,4F1,F93
0A: 000,000,000,000,000,000,000,000     1A: 000,000,000,000,000,000,000,000
0B: 000,000,000,000,000,000,000,000     1B: FAF,000,FF5,FF0,F40,5AF,024,0FF
0C: 000,000,000,000,000,000,000,000     1C: FAF,0FF,046,6D9,008,FF4,F30,300
0D: 000,000,000,000,000,000,000,000     1D: FAF,0F9,F7F,838,415,7BE,27D,048
0E: 000,000,000,000,000,000,000,000     1E: FAF,0FF,000,02F,F4F,5DF,888,766
0F: 000,000,000,000,000,000,000,000     1F: F9F,007,000,00F,0FF,027,FFF,FFF
```

The 100h palette entries are containing 42h different colors. The Palette data
is stored in three N82S129N chips (three 256x4bit PROMs): IC "6D"=Blue, IC
"6E"=Green, IC "6F"=Red.

**PC10 Character Sets - Available Characters per Color**

The PC10's character set ROM contains four character sets, all having
identically looking characters, but with different color numbers, and with
different amounts of defined characters:

```
Charset0:  0123456789.'!-",:+?()/=ABCDEFGHIJKLMNOPQRSTUVWXYZabc..xyz
Charset1:  0123456789 '!- ,   ()  ABCDEFGHIJKLMNOPQRSTUVWXYZabc..xyz
Charset2:  0123456789.'!-",:+     ABCDEFGHIJKLMNOPQRSTUVWXYZ
Charset3:              !          ABC E  HI  LMNOP RSTU   Y
```

Title strings must use Charset2 (and will be changed to Charset0 for the
currently selected title).

Instruction text can have the Charset ROM colors combined with different VRAM
palette attributes. To match up with the window border, the background color
should be dark green (as so in palette 10h-11h and also in 1Bh-1Eh). Possible
Charset vs Palette combinations for Instruction Texts are:

```
Palette:   10h       11h       1Bh       1Ch       1Dh       1Eh
Charset0:  White     Mint      D.Blue    Orange2   D.Green   White
Charset1:  Orange    L.Green   Blue      Magenta   P.Green   Orange3
Charset2:  Red       M.Green   White     Red       Pink      Red
Charset3:  Yellow    Yellow    -         -         -         -
```

Accordingly, there are several restrictions on coloring. For example, Red
cannot be used with lowercase letters.

**PC10 Z80 Video Circuit Timings - Dual Monitor Version**

The Z80 Video Circuit is producing roughly NTSC-style timings (the output is
passed straight to the built-in RGB monitor, so there has been no need to match
exact television standards).

```
Resolution 32x28 tiles of 8x8 pix each (=256x224 pixels)
Dotclk = 5.040MHz (X2 oscillator, 20.160MHz, divided by 4)
Dots per line = 327 total dots (256 visible dots, plus 71 h-blank dots)
Lines per frame = 256 total lines (224 visible lines, plus 32 v-blank lines)
VBlank at Y=F0h..0Fh (when V5=1, V6=1, V7=1, latched at raising V4)
Vsync at Y=F8h..FBh (when Vblank=1, V4=1, V3=1, V2=0)
Hsync when HBlank=1, H1=1, H2=0
Screen Border Color during H/V-Blanking is Color 0 of Palette 0 (Black)
Frame Rate = 60.20642202Hz  = 5.040MHz / 256 Lines / 327 Dots
Z80 NMI (if enabled) is generated on begin of Z80 Circuit's Vblank (Y=F0h)
```

The Z80 Video Circuit's frame rate is NOT synchronized with the NES PPU.

**PC10 Z80 Video Circuit Timings - Single Monitor Version**

Not much known here. There's no schematic for the Single Monitor version.
According to a component list, the X2 oscillator for the Z80 video circuit is
21.47727MHz (same as the X3 oscillator for the NES). Accordingly, one may
assume that dots/line and lines/frame are also matched to NES timings (else the
display would shake when switching between Z80 picture and NES picture).

**LED Character Set (74HC4511 BCD-driver with yellow-green GL-8E040 display)**

The Single-Monitor version is having a 4-Digit LED display for displaying the
remaining time during Game (and also in Menu). The LED Digits 00h..09h are as
so:

```
_         _    _         _         _    _    _
| |    |   _|   _|  |_|  |_   |_     |  |_|  |_|
|_|.   |. |_.   _|.   |.  _|. |_|.   |. |_|.   |.
```

Digit 06h/09h may vary from chip to chip: existing Playchoice cabinets are
drawing them as shown above (without upper/lower horizontal line, as specified
by Toshiba and Philips) (whilst Texas Instruments specifies them with extra
lines). The "." dot (eighth LED segment) is always off in the Playchoice.

PC10 NES-Side

The PC10 cartridges are usually containing PRG-ROM and CHR-ROM on EPROMs
(though, despite of the EPROM-storage, they are usually byte-identical to
normal NES ROM-cartridge versions). Nonetheless, there are a few things to
recurse when making PC10 games (and eventually customize NES games
accordingly):

**NES PPU Palette**

The PC10 uses a RGB palette, the colors are similar to NES Composite colors,
but not 100% same: PC10 colors are more colorful than the pastelized NES
colors. Pastel-cyan and two grayshades are completely missing. And, some colors
are having wrong intensities, so fade-in/fade-out effects programmed for the
NES can produce ugly flickering on the PC10. The color emphasis bits are also
working differently as on NES. The PC10 palette is same as the "standard" VS
System palette, for details see:

VS System PPUs and Palettes

**NES NMI Disable**

The PC10 BIOS is watching the NES NMI signal. If the NES disables NMIs for a
longer period (around 255 frames), then it'll treat the game to have locked up,
and will terminate the game. If that happens more than twice, then it will
additionally remove the game from the menu's cartridge list.

**NES Controller Disable**

The PC10 can disable the NES controllers in demo mode. For some reason, it
doesn't disable the serial shift-register outputs, but rather only the
Select/Start parallel inputs (and lightgun trigger). Accordingly, to prevent
the game being played in demo mode, starting the game should work ONLY via
Start button (ie. not via Button A/B).

**NES Controller Connection**

The PC10 has two "joypads" and one "zapper", unlike as on NES, it isn't
possible to disconnect them, so the game must work with that hardware connected
(which should be usually no problem).

Whereas, the lightgun seems to be an optional add-on, not installed in all
cabinets (especially not in tabletop cabinets).

The Start/Select buttons are mapped to the Joypad 1 input. The Joypad 2 input
doesn't have any such buttons (similar as japanese Famicom joypads).

**NES Pause / Timings**

It seems to be possible to "pause" the NES CPU. Unknown if this is actually
done during game-execution... if so, pausing/unpausing may get the CPU offsync
with the PPU, which might confuse & crash a few NES games. Initial CPU
Reset time vs PPU Reset time may be also different as on normal NES.

And, reportedly, the PC10 RGB-PPU timing isn't exactly same as NES NTSC-PPU
timing (reportedly something to do with missing dots on some NTSC scanlines or
so).

**NES IRQs**

On older PC10 mainboards, the /IRQ pin is wired as output (rather than as
input), making it incompatible with games that do use IRQs. The mainboards can
be upgraded (with two wires) to support IRQs. The upgrade may be found on many
mainboards as it's required for PCH1-01-ROM-G game cards.

**Communicating between NES and Z80 CPUs**

There is no intended CPU-to-CPU communication support. However, with some
trickery, it should be possible. On the Z80 side, one could place custom code
in the C9BEh function (which is called once every mainloop cycle). For
NES-to-Z80 transfers one could enable/disable NES NMIs to transfer 1bit per
frame. For Z80-to-NES transfers one could eventually issue specially timed CPU
or PPU Reset pulses.

PC10 Cabinet and BIOS Versions

**Dual-Monitor Upright Cabinets**

The upper monitor (for Z80 menu, instructions, and remaining time/credit
display) is usually smaller than the lower one (NES game picture). There seems
to be also a variant with two big monitors. Front panel is having 5 control
buttons.

**Single-Monitor Cabinets**

The BIOS can switch the monitor to display either the Z80 or NES picture. The
remaining credit/time is displayed via 4-digit 7-segment LED displays. Front
panel can have 4 or 5 control buttons. Switching the BIOS to the correct front
panel type seems to rely on inversion of the reset button signal, and seems to
work properly only on certain dip-switch settings.

**Single-Monitor Upright Cabinets**

These are usually upgraded VS System cabinets with a PC10 mainboard (either
original VS System cabinets, or even older cabinets that have been formerly
upgraded to become a VS System). The cabinets are having VS System style front
panels with only four buttons (named 1-4) instead of the normal five PC10
buttons (Channel Select, Enter, Reset, Start, Game Select).

**Single-Monitor Countertop/Sitdown Cabinets**

The Playchoice "countertop" version (a white box to be placed on a bar counter
or table) is a native PC10 cabinet (with 5 control buttons). The "red tent"
version (a red tent-shaped console with two legs) is an upgraded VS Dual System
sitdown version (with 4 control buttons).

**Official BIOSes (IC "8T")**

```
PCH1-C.8T   Dual-Monitor Version   (CRC32=D52FA07Ah) (16Kbytes)
PCK1-C.8T   Single-Monitor Version (CRC32=503EE8B1h) (16Kbytes)
```

There aren't any further known versions/revisions.

```
0BE8CEB4
```

**Homebrew BIOSes**

```
Oliver Achten's BIOS v0.1 (2002)   (CRC32=FB96DE76h) (8Kbytes)
gamehacker's BIOS v1.0             (CRC32=18B4E0B5h) (16Kbytes)
gamehacker's BIOS v1.1             (CRC32=80BD20EFh) (16Kbytes)
```

The homebrew BIOSes are allowing to play NES games without PROMs. They are
suffering some restrictions:

Oliver's BIOS doesn't have any support for Titles/Instructions (neither for
original nor homebrew games). The BIOS can be DIP-Switched to Single/Dual
Monitor mode. Unknown if coin inputs are supported.

Gamehacker's BIOSes are supporting Titles (from original games and homebrew
games; for the latter, the title can be stored in custom INST ROM format, or in
SRAM; which probably only lasts for some days/weeks?). Instructions aren't
supported (neither for original nor homebrew games). Unknown if this BIOS
supports both Single and Dual Monitor versions. The BIOS doesn't support coin
inputs (freeplay mode only)

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

Z80 Flags

**Flag Summary**

The Flags are located in the lower eight bits of the AF register pair.

```
Bit Name  Set  Clr  Expl.
0   C     C    NC   Carry Flag
1   N     -    -    Add/Sub-Flag (BCD)
2   P/V   PE   PO   Parity/Overflow-Flag
3   -     -    -    Undocumented
4   H     -    -    Half-Carry Flag (BCD)
5   -     -    -    Undocumented
6   Z     Z    NZ   Zero-Flag
7   S     M    P    Sign-Flag
```

**Carry Flag (C)**

This flag signalizes if the result of an arithmetic operation exceeded the
maximum range of 8 or 16 bits, ie. the flag is set if the result was less than
Zero, or greater than 255 (8bit) or 65535 (16bit). After rotate/shift
operations the bit that has been 'shifted out' is stored in the carry flag.

**Zero Flag (Z)**

Signalizes if the result of an operation has been zero (Z) or not zero (NZ).
Note that the flag is set (1) if the result was zero (0).

**Sign Flag (S)**

Signalizes if the result of an operation is negative (M) or positive (P), the
sign flag is just a copy of the most significant bit of the result.

**Parity/Overflow Flag (P/V)**

This flag is used as Parity Flag, or as Overflow Flag, or for other purposes,
depending on the instruction.

Parity: Bit7 XOR Bit6 XOR Bit5 ... XOR Bit0 XOR 1.

8bit Overflow: Indicates if the result was greater/less than +127/-128.

HL Overflow: Indicates if the result was greater/less than +32767/-32768.

After LD A,I or LD A,R: Contains current state of IFF2.

After LDI,LDD,CPI,CPD,CPIR,CPDR: Set if BC<>0 at end of operation.

**BCD Flags (H,N)**

These bits are solely supposed to be used by the DAA instruction. The N flag
signalizes if the previous operation has be an addition or substraction. The H
flag indicates if the lower 4 bits exceeded the range from 0-0Fh. (For 16bit
instructions: H indicates if the lower 12 bits exceeded the range from
0-0FFFh.)

After adding/subtracting two 8bit BCD values (0-99h) the DAA instruction can be
used to convert the hexadecimal result in the A register (0-FFh) back to BCD
format (0-99h). Note that DAA also requires the carry flag to be set correctly,
and thus should not be used after INC A or DEC A.

**Undocumented Flags (Bit 3,5)**

The content of these undocumented bits is filled by garbage by all instructions
that affect one or more of the normal flags (for more info read the chapter
Garbage in Flag Register), the only way to read out these flags would be to
copy the flags register onto the stack by using the PUSH AF instruction.

However, the existence of these bits makes the AF register a full 16bit
register, so that for example the code sequence PUSH DE, POP AF, PUSH AF, POP
HL would set HL=DE with all 16bits intact.

Z80 Load Commands

**8bit Load Commands**

```
Instruction    Opcode  Cycles Flags  Notes
ld   r,r       xx           4 ------ r=r
ld   i,i       pD xx        8 ------ i=i
ld   r,n       xx nn        7 ------ r=n
ld   i,n       pD xx nn    11 ------ i=n
ld   r,(HL)    xx           7 ------ r=(HL)
ld   r,(ii+d)  pD xx dd    19 ------ r=(ii+d)
ld   (HL),r    7x           7 ------ (HL)=r
ld   (ii+d),r  pD 7x dd    19 ------
ld   (HL),n    36 nn       10 ------
ld   (ii+d),n  pD 36 dd nn 19 ------
ld   A,(BC)    0A           7 ------
ld   A,(DE)    1A           7 ------
ld   A,(nn)    3A nn nn    13 ------
ld   (BC),A    02           7 ------
ld   (DE),A    12           7 ------
ld   (nn),A    32 nn nn    13 ------
ld   A,I       ED 57        9 sz0i0- A=I  ;Interrupt Register
ld   A,R       ED 5F        9 sz0i0- A=R  ;Refresh Register
ld   I,A       ED 47        9 ------
ld   R,A       ED 4F        9 ------
```

**16bit Load Commands**

```
Instruction    Opcode  Cycles Flags  Notes
ld   rr,nn     x1 nn nn    10 ------ rr=nn    ;rr may be BC,DE,HL or SP
ld   ii,nn     pD 21 nn nn 13 ------ ii=nn
ld   HL,(nn)   2A nn nn    16 ------ HL=(nn)
ld   ii,(nn)   pD 2A nn nn 20 ------ ii=(nn)
ld   rr,(nn)   ED xB nn nn 20 ------ rr=(nn)  ;rr may be BC,DE,HL or SP
ld   (nn),HL   22 nn nn    16 ------ (nn)=HL
ld   (nn),ii   pD 22 nn nn 20 ------ (nn)=ii
ld   (nn),rr   ED x3 nn nn 20 ------ (nn)=rr  ;rr may be BC,DE,HL or SP
ld   SP,HL     F9           6 ------ SP=HL
ld   SP,ii     pD F9       10 ------ SP=ii
push rr        x5          11 ------ SP=SP-2, (SP)=rr  ;rr may be BC,DE,HL,AF
push ii        pD E5       15 ------ SP=SP-2, (SP)=ii
pop  rr        x1          10 (-AF-) rr=(SP), SP=SP+2  ;rr may be BC,DE,HL,AF
pop  ii        pD E1       14 ------ ii=(SP), SP=SP+2
ex   DE,HL     EB           4 ------ exchange DE  HL
ex   AF,AF     08           4 xxxxxx exchange AF  AF'
exx            D9           4 ------ exchange BC,DE,HL  BC',DE',HL'
ex   (SP),HL   E3          19 ------ exchange (SP)  HL
ex   (SP),ii   pD E3       23 ------ exchange (SP)  ii
```

**Blocktransfer**

```
Instruction    Opcode  Cycles Flags  Notes
ldi            ED A0       16 --0e0- (DE)=(HL), HL=HL+1, DE=DE+1, BC=BC-1
ldd            ED A8       16 --0e0- (DE)=(HL), HL=HL-1, DE=DE-1, BC=BC-1
ldir           ED B0  bc*21-5 --0?0- ldi-repeat until BC=0
lddr           ED B8  bc*21-5 --0?0- ldd-repeat until BC=0
```

Z80 Rotate/Shift and Singlebit Operations

**Rotate and Shift Commands**

```
Instruction    Opcode  Cycles Flags  Notes
rlca           07           4 --0-0c rotate akku left
rla            17           4 --0-0c rotate akku left through carry
rrca           0F           4 --0-0c rotate akku right
rra            1F           4 --0-0c rotate akku right through carry
rld            ED 6F       18 sz0p0- rotate left low digit of A through (HL)
rrd            ED 67       18 sz0p0- rotate right low digit of A through (HL)
r        CB xx        8 sz0p0c see below
(HL)     CB xx       15 sz0p0c see below
(ii+d)   pD CB dd xx 23 sz0p0c see below
r,(ii+d) pD CB dd xx 23 sz0p0c see below, UNDOCUMENTED modify and load
```

Whereas <cmd> may be:

```
rlc    rotate left
rl     rotate left through carry
rrc    rotate right
rr     rotate right through carry
sla    shift left arithmetic (b0=0)
sll    UNDOCUMENTED shift left (b0=1)
sra    shift right arithmetic (b7=b7)
srl    shift right logical (b7=0)
```

**Singlebit Operations**

```
Instruction    Opcode  Cycles Flags  Notes
bit  n,r       CB xx        8 xz1x0- test bit n  ;n=0..7
bit  n,(HL)    CB xx       12 xz1x0-
bit  n,(ii+d)  pD CB dd xx 20 xz1x0-
set  n,r       CB xx        8 ------ set bit n   ;n=0..7
set  n,(HL)    CB xx       15 ------
set  n,(ii+d)  pD CB dd xx 23 ------
set r,n,(ii+d) pD CB dd xx 23 ------ UNDOCUMENTED set n,(ii+d) and ld r,(ii+d)
res  n,r       CB xx        8 ------ reset bit n ;n=0..7
res  n,(HL)    CB xx       15 ------
res  n,(ii+d)  pD CB dd xx 23 ------
res r,n,(ii+d) pD CB dd xx 23 ------ UNDOCUMENTED res n,(ii+d) and ld r,(ii+d)
ccf            3F           4 --h-0c h=cy, cy=cy xor 1
scf            37           4 --0-01 cy=1
```

Z80 I/O Commands

```
Instruction    Opcode  Cycles Flags  Notes
in   A,(n)     DB nn       11 ------ A=PORT(A*100h+n)
in   r,(C)     ED xx       12 sz0p0- r=PORT(BC)
in   (C)       ED 70       12 sz0p0- **undoc/illegal** VOID=PORT(BC)
out  (n),A     D3 nn       11 ------ PORT(A*100h+n)=A
out  (C),r     ED xx       12 ------ PORT(BC)=r
out  (C),0     ED 71       12 ------ **undoc/illegal** PORT(BC)=00
ini            ED A2       16 xexxxx MEM(HL)=PORT(BC), HL=HL+1, B=B-1
ind            ED AA       16 xexxxx MEM(HL)=PORT(BC), HL=HL-1, B=B-1
outi           ED A3       16 xexxxx B=B-1, PORT(BC)=MEM(HL), HL=HL+1
outd           ED AB       16 xexxxx B=B-1, PORT(BC)=MEM(HL), HL=HL-1
inir           ED B2   b*21-5 x1xxxx same than ini, repeat until b=0
indr           ED BA   b*21-5 x1xxxx same than ind, repeat until b=0
otir           ED B3   b*21-5 x1xxxx same than outi, repeat until b=0
otdr           ED BB   b*21-5 x1xxxx same than outd, repeat until b=0
```

Z80 Meaningless and Duplicated Opcodes

**Mirrored Instructions**

NEG  (ED44) is mirrored to ED4C,54,5C,64,6C,74,7C.

RETN (ED45) is mirrored to ED55,65,75.

RETI (ED4D) is mirrored to ED5D,6D,7D.

**Mirrored IM Instructions**

IM 0,X,1,2 (ED46,4E,56,5E) are mirrored to ED66,6E,76,7E.

Whereas IM X is an undocumented mirrored instruction itself which appears to be
identical to either IM 0 or IM 1 instruction (?).

**Duplicated LD HL Instructions**

LD (nn),HL (opcode 22NNNN) is mirrored to ED63NNNN.

LD HL,(nn) (opcode 2ANNNN) is mirrored to ED6BNNNN.

Unlike the other instructions in this chapter, these two opcodes are officially
documented. The clock/refresh cycles for the mirrored instructions are then
20/2 instead of 16/1 as for the native 8080 instructions.

**Mirrored BIT N,(ii+d) Instructions**

Unlike as for RES and SET, the BIT instruction does not support a third
operand, ie. DD or FD prefixes cannot be used on a BIT N,r instruction in order
to produce a BIT r,N,(ii+d) instruction. When attempting this, the 'r' operand
is ignored, and the resulting instruction is identical to BIT N,(ii+d).

Except that, not tested yet, maybe undocumented flags are then read from 'r'
instead of from ii+d(?).

**Non-Functional Opcodes**

The following opcodes behave much like the NOP instruction.

ED00-3F, ED77, ED7F, ED80-9F, EDA4-A7, EDAC-AF, EDB4-B7, EDBC-BF, EDC0-FF.

The execution time for these opcodes is 8 clock cycles, 2 refresh cycles.

Note that some of these opcodes appear to be used for additional instructions
by the R800 CPU in newer turbo R (MSX) models.

**Ignored DD and FD Prefixes**

In some cases, DD-prefixes (IX) and FD-prefixes (IY) may be ignored by the CPU.
This happens when using one (or more) of the above prefixes prior to
instructions that already contain an ED, DD, or FD prefix, or prior to any
instructions that do not support IX, IY, IXL, IXH, IYL, IYH operands. In such
cases, 4 clock cycles and 1 refresh cycle are counted for each ignored prefix
byte.

Z80 Compatibility

The Z80 CPU is (almost) fully backwards compatible to older 8080 and 8085 CPUs.

**Instruction Format**

The Z80 syntax simplifies the chaotic 8080/8085 syntax. For example, Z80 uses
the command "LD" for all load instructions, 8080/8085 used various different
commands depending on whether the operands are 8bit registers, 16bit registers,
memory pointers, and/or an immediates. However, these changes apply to the
source code only - the generated binary code is identical for both CPUs.

**Parity/Overflow Flag**

The Z80 CPU uses Bit 2 of the flag register as Overflow flag for arithmetic
instructions, and as Parity flag for other instructions. 8080/8085 CPUs are
always using this bit as Parity flag for both arithmetic and non-arithmetic
instructions.

**Z80 Specific Instructions**

The following instructions are available for Z80 CPUs only, but not for older
8080/8085 CPUs:

All CB-prefixed opcodes (most Shift/Rotate, all BIT/SET/RES commands).

All ED-prefixed opcodes (various instructions, and all block commands).

All DD/FD-prefixed opcodes (registers IX and IY).

As well as DJNZ nn; JR nn; JR f,nn; EX AF,AF; and EXX.

**8085 Specific Instructions**

The 8085 instruction set includes two specific opcodes in addition to the 8080
instruction set, used to control 8085-specifc interrupts and SID and SOD
input/output signals. These opcodes, RIM (20h) and SIM (30h), are not supported
by Z80/8080 CPUs.

**Z80 vs Z80A**

Both Z80 and Z80A are including the same instruction set, the only difference
is the supported clock frequency (Z80 = max 2.5MHz, Z80A = max 4MHz).

**NEC-780 vs Zilog-Z80**

These CPUs are apparently fully compatible to each other, including for
undocumented flags and undocumented opcodes.

Z80 Local Usage

**Playchoice 10 (Z80)**

Clocked at 4.000MHz.

NMIs are triggered on Vblank start (of the Z80 video circuit).

Normal interrupts are not used. There is no DRAM (no Refresh used).

There is memory mapped I/O at E000h..FFFFh. Writes to that area are done by LD
[RR],R and LD [RR],NN and PUSH RR opcodes. Reads from that area are done by BIT
N,[HL], XOR A,[HL], and JP HL opcodes (JP HL is executing a RST NN opcode in
the memory mapped I/O area).
