# Pictureprocessingunitppu

> Source: https://problemkaputt.de/everynes.htm
> Section: Pictureprocessingunitppu

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

Based on "Nintendo Entertainment System Documentation" Version 2.00 by Jeremy
Chadwick aka Y0SHi aka JDC. Which was itself based on "Nintendo Entertainment
System Architecture" by Marat Fayzullin.

3D Glasses

PPU Control and Status Registers

**2000h - PPU Control Register 1 (W)**

```
Bit7  Execute NMI on VBlank             (0=Disabled, 1=Enabled)
Bit6  PPU Master/Slave Selection        (0=Master, 1=Slave) (Not used in NES)
Bit5  Sprite Size                       (0=8x8, 1=8x16)
Bit4  Pattern Table Address Background  (0=VRAM 0000h, 1=VRAM 1000h)
Bit3  Pattern Table Address 8x8 Sprites (0=VRAM 0000h, 1=VRAM 1000h)
Bit2  Port 2007h VRAM Address Increment (0=Increment by 1, 1=Increment by 32)
Bit1-0 Name Table Scroll Address        (0-3=VRAM 2000h,2400h,2800h,2C00h)
(That is, Bit0=Horizontal Scroll by 256, Bit1=Vertical Scroll by 240)
```

**2001h - PPU Control Register 2 (W)**

```
Bit7-5 Color Emphasis       (0=Normal, 1-7=Emphasis) (see Palettes chapter)
Bit4  Sprite Visibility     (0=Not displayed, 1=Displayed)
Bit3  Background Visibility (0=Not displayed, 1=Displayed)
Bit2  Sprite Clipping       (0=Hide in left 8-pixel column, 1=No clipping)
Bit1  Background Clipping   (0=Hide in left 8-pixel column, 1=No clipping)
Bit0  Monochrome Mode       (0=Color, 1=Monochrome)  (see Palettes chapter)
```

If both sprites and BG are disabled (Bit 3,4=0) then video output is disabled,
and VRAM can be accessed at any time (instead of during VBlank only). However,
SPR-RAM does no longer receive refresh cycles, and its content will gradually
degrade when the display is disabled.

**2002h - PPU Status Register (R)**

```
Bit7   VBlank Flag    (1=VBlank)
Bit6   Sprite 0 Hit   (1=Background-to-Sprite0 collision)
Bit5   Lost Sprites   (1=More than 8 sprites in 1 scanline)
Bit4-0 Not used       (Undefined garbage)
```

Reading resets the 1st/2nd-write flipflop (used by Port 2005h and 2006h).

Reading resets Bit7, can be used to acknowledge NMIs, Bit7 is also
automatically reset at the end of VBlank, so manual acknowledge is normally not
required (unless one wants to free the NMI signal for external NMI inputs) (and
unless one wants to disable/reenable NMIs during NMI handling, in that case
Bit7 MUST be acknowledge before reenabling NMIs, else NMI would be executed
another time).

**Status Notes**

VBlank flag is set in each frame, even if the display is fully disabled, and
even if NMIs are disabled. Hit flag may become set only if both BG and OBJ are
enabled. Lost Sprites flag may become set only if video is enabled (ie. BG or
OBJ must be on). For info about the "Not used" status bits, and some other PPU
bits see:

Unpredictable Things

**VS System**

Some VS System PPUs have Port 2000h/2001h swapped, and do have a Chip ID in
LSBs of 2002h. For details, see

VS System

PPU VRAM Access Registers

Registers used to Read and Write VRAM data, and for Background Scrolling.

The CPU can Read/Write VRAM during VBlank only - because the PPU permanently
accesses VRAM during rendering (even in HBlank phases), and because the PPU
uses the VRAM Address register as scratch pointer. Respectively, the address in
Port 2006h is destroyed after rendering, and must be re-initialized before
using Port 2007h.

**1st/2nd Write**

Below Port 2005h and 2006h require two 8bit writes to receive a 16bit
parameter, the current state (1st or 2nd write) is memorized in a single
flipflop, which is shared for BOTH Port 2005h and 2006h. The flipflop is reset
when reading from PPU Status Register Port 2002h (the next write will be then
treated as 1st write) (and of course it is also reset after any 2nd write).

**2005h - PPU Background Scrolling Offset (W2)**

Defines the coordinates of the upper-left background pixel, together with PPU
Control Register 1, Port 2000h, Bits 0-1).

```
Port 2005h-1st write: Horizontal Scroll Origin (X*1) (0-255)
Port 2005h-2nd write: Vertical Scroll Origin   (Y*1) (0-239)
Port 2000h-Bit0: Horizontal Name Table Origin  (X*256)
Port 2000h-Bit1: Vertical Name Table Origin    (Y*240)
```

Caution: The above scroll reload settings are overwritten by writes to Port
2006h. See PPU Scrolling chapter for more info.

**2006h - VRAM Address Register (W2)**

Used to specify the 14bit VRAM Address for use with Port 2007h.

```
Port 2006h-1st write: VRAM Address Pointer MSB (6bit)
Port 2006h-2nd write: VRAM Address Pointer LSB (8bit)
```

Caution: Writes to Port 2006h are overwriting scroll reload bits (in Port 2005h
and Bit0-1 of Port 2000h). And, the PPU uses the Port 2006h register internally
during rendering, when the display is enabled one should thus reinitialize Port
2006h at begin of VBlank before accessing VRAM via Port 2007h.

**2007h - VRAM Read/Write Data Register (RW)**

The PPU will auto-increment the VRAM address (selected via Port 2006h) after
each read/write from/to Port 2007h by 1 or 32 (depending on Bit2 of $2000).

```
Bit7-0  8bit data read/written from/to VRAM
```

Caution: Reading from VRAM 0000h-3EFFh loads the desired value into a latch,
and returns the OLD content of the latch to the CPU. After changing the address
one should thus always issue a dummy read to flush the old content. However,
reading from Palette memory VRAM 3F00h-3FFFh, or writing to VRAM 0000-3FFFh
does directly access the desired address.

Note: Some (maybe all) RGB PPUs (as used in Famicom Titler) do not allow to
read palette memory (instead, they appear to mirror 3Fxxh to 2Fxxh).

**Caution**

The APU (if DMC sound is used) can conflict with joypad reads! For details,
see:

APU DMC-DMA Glitch

PPU Tile Memory

PPU 0000h-0FFFh - Pattern Table 0 (4K) (256 Tiles)

PPU 1000h-1FFFh - Pattern Table 1 (4K) (256 Tiles)

**Pattern Table Format**

Each pattern table contains 256 tiles. When using both pattern table 0 and 1,
up to 512 tiles can be used for Background and Sprites.

Each tile consists of a 8x8 pixel bitmap with 2bit depth (4 colors). Each tile
occopies 16 bytes, the first 8 bytes contain color bit 0 for each pixel, the
next 8 bytes color bit 1. Each byte defines a row of 8 pixels (MSB left).

**Pattern Table Memory**

The console does NOT include built-in Pattern Table Memory. Instead, Pattern
tables are located in the cartridge, usually in a separate ROM chip, or (less
often) in a SRAM chip. Cartridges with more than 8K Pattern memory may contain
whatever mapping mechanisms to map the memory into the PPU 8K Pattern Memory
area.

There are some special mappers that do automatically change the CHR banks
during rendering (allowing to access more than 8K in one frame):

Mapper 5: MMC5 - BANKING, IRQ, SOUND, VIDEO, MULTIPLY, etc.

Mapper 9: MMC2 - PRG/24K/8K, VROM/4K, NT, LATCH

Mapper 10: MMC4 - PRG/16K, VROM/4K, NT, LATCH

Mapper 96: 74161/32 - PRG/32K, CHR/16K/4K, LATCH

Cartridges with CHR-RAM (instead CHR-ROM) usually have 8K RAM, but there also a
few with 16K, 32K or 64K RAM:

Mapper 13: CPROM - 16K VRAM

Mapper 96: 74161/32 - PRG/32K, CHR/16K/4K, LATCH

Mapper 168: RacerMate PRG/16K, VRAM/4K, IRQ

PPU Sprites

SPR-RAM 00-FF - Sprite Attributes (256 bytes, for 64 sprites / 4 bytes each)

The PPU supports 64 sprites, which can be either 8x8 or 8x16 pixels in size,
only 8 sprites can be displayed per scanline. The sprite Tile bitmaps are kept
within the Pattern Table region of VRAM (which is also used for BG Tiles).

Sprite-RAM is built-in in the PPU-chip, and can be accessed via I/O ports only
(it is not part of the PPU or CPU memory area). Each four bytes in SPR-RAM
define attributes for one sprite, bytes 0-3 for sprite 0, up to bytes FCh-FFh
for sprite 63.

**SPR-RAM Byte 0 - Y Coordinate Minus 1**

```
Vertical Position-1 (FFh,00h..EEh=Scanline 0..239, EFh..FEh=Not displayed)
```

The sprites can be moved bottom-offscreen, but cannot be moved top-offscreen.

**SPR-RAM Byte 1 - Tile Number**

In 8x8 pixel mode (PPU Control Register 1, Bit5=0):

```
Bit7-0  Specifies 8bit tile number
And,    Pattern Table selected by Bit 3 in PPU Control Register 1
```

In 8x16 pixel mode (PPU Control Register 1, Bit5=1):

```
Bit7-1  Upper 7bit of tile number (N=0-127 uses Tiles N*2 and N*2+1)
Bit0    Pattern Table Address     (0=VRAM 0000h, 1=VRAM 1000h)
```

**SPR-RAM Byte 2 - Attributes**

```
7    Vertical Flip        (0=Normal, 1=Mirror)
6    Horizontal Flip      (0=Normal, 1=Mirror)
5    Background Priority  (0=Sprite in front of BG, 1=Sprite Behind BG)
4-2  Not used             (Always zero when reading from SPR-RAM)
1-0  Sprite Palette       (0-3=Sprite Palette 0-3)
```

**SPR-RAM Byte 3 - X Coordinate**

```
Horizontal Position (00h..FFh)
```

Sprites can be moved right-offscreen, and clipping via Port 2001h also allows
to move sprites somewhat left-offscreen.

**Sprite Priorites**

If two or more non-transparent sprite-pixels overlap, then only the sprite with
highest priority is processed. If the sprites background priority bit is set to
"Behind BG", then it will be hidden behind any non-transparent background
pixels.

```
Sprite 0 = highest priority
Sprite 63 = lowest priority
```

Mind that the PPU processes ONLY the sprite with highest priority, eg. if a
non-transparent pixel of sprite 5 hides "Behind BG", then sprite 6-63 won't be
displayed (even if they are "In Front of BG").

**Sprite 0 Hit Flag (Collision Check between BG and Sprite 0)**

The Hit Flag, Bit 6 of register 2002h, gets set when the cathode ray beam
passes a non-transparent Sprite 0 pixel which is overlapping a non-transparent
BG pixel (regardless the sprites BG priority).

The Hit Flag is automatically reset at the end of the VBlank period, it cannot
be set or reset by software, that means one can detect only one Hit per frame.

Aside from a normal collision detection, the Hit Flag is also useful to detect
when the cathode ray beam has reached a specific screen location, eg. to split
the picture into a scrolled and non-scrolled section.

Color 1 or color 2 are (NOT) non-nontransparent (?)

PPU Dimensions & Timings

Below are timings for Nintendo's NTSC and PAL consoles, and for the Dendy (a
russian Famicom clone with PAL output and Famicom-like NTSC-style timings).

**Clock Speeds**

```
Type               NTSC               PAL                 Dendy
Master Clock (X1)  21.47727MHz        26.6017125MHz       Like PAL
Color Clock        3.579545MHz=X1/6   4.43361875MHz=X1/6  Like PAL
Dot Clock          5.3693175MHz=X1/4  5.3203425MHz=X1/5   Like PAL
CPU Clock          1.7897725MHz=X1/12 1.66260703MHz=X1/16 1.773448MHz=X1/15
Frame Rate         60.09914261Hz      50.00697891Hz       Like PAL
```

Note: Above NTSC values are rounded. The exact NTSC Color Clock should be
315/88MHz.

**Vertical Timings (in scanlines)**

```
NTSC PAL  Dendy
?    1    ?           Pre-Render Time
240? 239  240?        Rendering Time
1    1    51?         Post-Render Time
20   70   20?         Vblank
1    1    1?          Post-Blank Time
---- ---- ----        -------
262  312  312?        Total number of Scanlines
```

**Horizontal Timings (in dots)**

```
NTSC PAL  Dendy
25x  252  25x         Rendering Time
xx   89   xx          Hblank
---- ---- ----        -------
341  341  341?        Total number of dots per scanline
```

In each second frame, NTSC does output a short scanline with only 340 dots,
this happens only if BG and/or OBJ are enabled.

**CPU vs PPU Timings**

```
Type                       NTSC            PAL             Dendy
Dots per CPU Clk           3.0 (12/4)      3.2 (16/5)      3.0 (15/5)
CPU Clks per Scanline      113.6666        106.5625
CPU Clks per One Frame     29780.66        33247.5
CPU Clks per Other Frame   29780.33        33247.5
CPU Clks per Two Frames    59561.0         66495.0
```

**Modulator vs PPU Timings**

```
Type                       NTSC            PAL             Dendy
Dots per Color Clk         1.5 (6/4)       1.2 (6/5)       1.2 (6/5)
Color Clks per Scanline    227.3333        284.1666
Color Clks per One Frame   59561.33        88660.0
Color Clks per Other Frame 59560.66        88660.0
Color Clks per Two Frames  119122.0        177320.0
```

**Visible Screen Resolution**

The logical screen resolution processed by the PPU is 256x240 pixels. However
the visible screen resolution is somewhat smaller, due to improper blanking
periods, and eventually due to exceeding the physical dimensions of (NTSC)
displays.

On PAL hardware, the upper 1 scanline, the left 2 pixels, and the right 2
pixels are invisible (displayed as black border). On NTSC hardware, the upper 8
scanlines, and the lower 8 scanlines are reportedly often invisible (224 lines
visible), eventually some NTSC screens are hiding only the upper 3 scanlines
(237 lines visible).

To be compatible with all types of displays, output valid 256x240 pixels, but
have the relevant information in the <middle> 240x224 pixels (30x28
tiles) only.

**Synchronizing Software with the Cathode Ray Beam**

The PPU sets the VBlank flag in PPU Status Register (and optionally produces an
NMI) once per frame. It doesn't support scanline interrupts, or a current
scanline number register, which is making it a bit difficult to access PPU
registers at specific Cathode Ray Beam locations (for example, to split the the
screen into two sections with different colors or scroll offsets). However, a
couple of methods could be used for that purpose:

```
1) Delay Loops synchronized with NMI (badly wasting CPU time) or using
meaningful code with fixed non-conditional execution time instead delays.
2) Producing a "Sprite 0 Hit", or a "More Than 8 Sprites Per Scanline"
situation at specific screen location (which sets corresponding flag in
PPU Status Register, one cannot reset the flag manually, so either works
only once per frame)
3) Using PCM Sound IRQs as Timer (synchronized with NMI)
4) Using external Timers (contained in some Cartridge Mappers)
```

The APU also contains a so-called "Frame" counter - that counter is NOT
physically synchronized with the PPU, and it doesn't even match the exact
number of clock cycles per frame. There's a limited chance that one could
program it to produce an IRQ at a specific screen location (and to
resynchronize it for the next frame).

**More detailed Timing Info**

PPU 2C02 Timings

**PAL Vblank Start Timing**

```
Line
/NMI   ---------------------------------------__________________________
Video: PPPPPPPPP-_C-PPPPPPPPP-_C-----------_C-----------_C-----------_C-
```

**PAL Vblank Middle Timing**

```
Line
/NMI   _________________________________________________________________
Video: ----------_C-----------____________-____________-____________-_C-
```

**PAL Vblank End Timing**

```
Line
/NMI   _____________----------------------------------------------------
Video: ----------_C-----------_C-----------_C-PPPPPPPPP-_C-PPPPPPPPP-_C-
```

**Whereas**

For Video: P=Picture, -=Blank, _=Sync, C=ColorBurst

/NMI is toggled on & off at the begin of the line (after the ending "-_C-"
blanking part of the previous line).

VSYNC is toggled at HSYNC, while active, it changes the "-_C-" part to "-___",
and also changes the next lines "picture" part from "---" to "___".

3D Glasses

**Famicom 3D System (Japan) (LCD shutter glasses)**

Japanese Famicom games are using fairly expensive LCD shutter glasses,
controlled via the 15pin Controller Expansion Port. Left & Right images are
transferred in separate frames (and the left/right shutters are opened/closed
accordingly).

```
Write to [4016h].Bit1   (0=Shut Which? Eye, 1=Shut WhichOther? Eye)
```

The advantage is that the 3D picture is fully colored. The downside is that the
hardware is expensive, and must be purchased separately. Another restriction is
that it requires the old 15pin Famicom connector (or an adapter for newer
Famicom and NES 7pin controller ports).

**Simple 3D Glasses (US and Europe) (red/cyan glasses)**

US and European NES games are using much cheaper "passive" 3D glasses with
colored "lens" instead of LCD shutters. Theoretically, this would allow to
transfer the left & right picture in one frame, but that'd require more CPU
load and color depth, so, in practice the games are drawing the left/right
frames separately (similar as in the LCD shutter method).

```
Right Eye -- Red
Left Eye  -- Blue/cyan or so
```

The advantage is that the hardware is so cheap that it can have been included
with the game for free. Cheapest variant would be "cardboard" glasses, though
there seems to have been also a yellow stylish plastic variant.

The downside of mis-using colors for 3D effects is that the picture will appear
more or less monochrome.

**Pulfrich Effect 3D Glasses (US) (dark/clear glasses)**

Another approach is using the Pulfrich effect with dark/clear glasses, this is
some sort psychophysical stuff, related to different signal timings for
bright/dark colors.

```
Right Eye -- Dark        ;\as so for NES Orb-3D
Left Eye  -- Clear       ;/(that is, opposite as for SNES Jim Power)
```

The advantage is that it's cheap, and that it can be even used for colored
images (unlike the red/cyan glasses method). The disadvantage is that it does
work only with permanently moving objects.

**Games with support for 3D Glasses**

```
Attack Animal Gakuen (J) (Pony Canyon)
Cosmic Epsilon (J) (Asmik)
Falsion (Konami)
Famicom Grand Prix: 3D Hot Rally (Nintendo)
Highway Star (J) aka Rad Racer (U) (E) (Square)
Tobidase Daisakusen (J) aka 3-D World Runner (U) (Square)
JJ Tobidase Daisakusen Part II (J) (Square)
Orb-3D (U) (Pulfrich Effect)
```

In most of that games, 3D mode is activated by pressing SELECT button. Game
versions that were released outside of Japan are customized to work with the
red/cyan glasses (or dark/clear glasses for Orb-3D).
