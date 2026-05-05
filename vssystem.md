# Vssystem

> Source: https://problemkaputt.de/everynes.htm
> Section: Vssystem

VS System
Nintendo's VS Systems are arcade machines with same chipset as NES consoles.
There are two versions:

```
VS UniSystem   --> 1-2 players (1 Monitor,  1 CPU,  1 PPU,  1 ROM-Set)
VS DualSystem  --> 2-4 players (2 Monitors, 2 CPUs, 2 PPUs, 2 ROM-Sets)
```

The DualSystem essentially contains two NES consoles that can be linked
together (or optionally can be used as two independend consoles, each one
running a different game).

[VS System Controllers](#vssystemcontrollers)

[VS System Games](#vssystemgames)

[VS System PPUs and Palettes](#vssystemppusandpalettes)

[VS System Protections](#vssystemprotections)

[Mapper 99: VS Unisystem Port 4016h - VROM/8K, (PRG/8K)](./cartridgesandmappers.md#mapper99vsunisystemport4016hvrom8kprg8k)

## VS System Games

**VS System Games**

```
PPU         Mapper   Title
*RP2C04-0001 MMC3+?   Atari RBI Baseball
RP2C04-0003 DUAL     Balloon Fight (... Dualsystem only?)
RP2C04-0001 DUAL     Baseball (... Dualsystem only?)
*RP2C04-0001 -        Battle City
RP2C04-0002 UNROM    Castlevania
RP2C04-0004 -        Clu Clu Land
RP2C04-0003 MMC1     Dr. Mario
RC2C03B     -        Duck Hunt (Lightgun) (one version)
RC2C03C     -        Duck Hunt (Lightgun) (other/same version)
RP2C04-0003 -        Excite Bike (instructions in demo-mode) (Nintendo) 1984
RP2C04-0004 -        Excite Bike (status-bar in demo-mode) (Nintendo) 1984
RP2C04-0001 MMC3     Freedom Force (Lightgun) (1988 Sunsoft)
RP2C04-0002 -        Golf
RC2C03B ?   -        Golf (Japan version?) XXX doesn't match ANY palette??
RP2C04-0003 VRC1     Goonies
RP2C04-0001 VRC1     Gradius
RC2C05-03   40K+16K  Gumshoe (Lightgun)
RP2C04-0001 -        Hogan's Alley (Lightgun)
RP2C04-0004 -        Ice Climber (Unisystem)
RP2C04-0004 DUAL     Ice Climber (Dualsystem) (JAPAN) "splitscreen-vertical"
RP2C04-0002 -        Ladies Golf
RP2C04-0002 -        Mach Rider (Endurance Course version)
RP2C04-0001 -        Mach Rider (Fighting Course version) (Japan version)
RC2C03B     DUAL     Mahjang (... Dualsystem only?)
RC2C05-02   -        Mighty Bomb Jack (Japan)
RC2C05-01   -        Ninja Jajamaru Kun (Japan)
RP2C04-0001 -        Pinball
RC2C03B     -        Pinball (Japan)
RP2C04-0001 Sunsoft3 Platoon
RP2C04-0002 DUMMY    Raid on Bungeling Bay (Japan)
RP2C04-0002 -        Slalom
RP2C04-0002 -        Soccer (japan version)
RP2C04-0003 -        Soccer (other version)
*RC2C03B     -        Star Luster
RP2C04-0004 -        Super Mario Bros.
RP2C04-0004 -        Super Mario Bros. (alternate version)
RP2C04-0004 -        Super Mario Bros. (Super Skater/Skate Kids CHR-ROM hack)
*RP2C04-0001 MMC3     Super Sky Kid
*RP2C04-0001 MMC3+?   Super Xevious
RC2C03B     DUAL     Tennis (... Dualsystem only?)
*RP2C04-0001 -        Tetris (by Tengen)
*RP2C04-0003 MMC3+?   TKO Boxing
RC2C05-04   UNROM    Top Gun
RP2C04-0002 DUAL     Wrecking Crew (... Dualsystem only?)
RC2C05-05   ?        (this PPU is used by which game ???) (does it exist?)
RP2C03B     ?        (this PPU is used by Playchoice 10; not by VS System)
RP2C03G     ?        (this PPU is used by which game ???) (does it exist?)
```

Note: The "*" marked entries are third-party games that do support different
PPUs (typically selected via DIP switch 6-8, the PPUs listed in the above table
are used when all DIP switches are OFF).

**VS System Mappers**

Below should be the correct mapper numbers used by VS games. Observe that
ROM-images do often contain incorrect mapper numbers (probably because there is
no automatic tool for dumping the VS ROM-Sets) (in worst case, the .NES header
is even missing the VS-flag).

```
-        Mapper 99 (standard VS System mapper; games without daughterboard)
40K+16K  Mapper 99 (with some extension to access 40K PRG-ROM)
DUAL     Mapper 99 (Dualsystem, requires two ROM-sets, two CPUs/PPUs etc.)
DUMMY    Mapper 99 (single-player, but requires Dummy PRG-ROM on second CPU)
MMC1     Mapper 1  (MMC1, or actually some pre-MMC1 74xxx-logic)
MMC3     Mapper 4  (MMC3, or actually some third-party pre-MMC3 chip)
MMC3+?   Mapper 4  (plus protection chip at 5Exxh or 5xxxh)
Sunsoft3 Mapper 67 (Sunsoft-3 chip)
UNROM    Mapper 2  (UNROM)
VRC1     Mapper 75 (Konami VRC1 chip)
```

The "Mapper 99" games consist of several 8Kbyte EPROMs (to be mounted directly
on the mainboard). The other games use special daughterboards (to be mounted
between CPU/PPU and mainboard).

The "MMC3" chips are actually some sort of pre-MMC3 28pin Shrink-DIP chips from
Namco, with part number "108 JAPAN", so correct mapper number would be "Mapper
206" or so?

**VS System PPUs**

```
RC2C03B (standard palette)                    ;\standard palette
RC2C03C (standard palette)                    ;/
RC2C05-01 (with ID ([2002h] AND xxh)=?)       ;\
RC2C05-02 (with ID ([2002h] AND 3Fh)=3Dh)     ; standard palette, but with
RC2C05-03 (with ID ([2002h] AND 1Fh)=1Ch)     ; swapped port 2000h/2001h,
RC2C05-04 (with ID ([2002h] AND 1Fh)=1Bh)     ; and Chip ID in port 2002h
RC2C05-05 (with ID ([2002h] AND xxh)=?)       ;/
RP2C04-0001 (special palette 1)               ;\
RP2C04-0002 (special palette 2)               ; special palettes
RP2C04-0003 (special palette 3)               ;
RP2C04-0004 (special palette 4)               ;/
```

For reference, below are some more RGB PPUs (although not used in VS System):

```
RP2C03B (standard palette) ;

## VS System Protections

**Unprotected Games**

VS Games are consisting of sets of EPROMs without any special hardware (apart
from custom cabinet front plates), the are no CIC lockout chips. That (and the
relative high price of the games) made it quite inviting to upgrade the
cabinets with illegal copies of newer games, or with unlicensed third-party
games.

**Nintendo Protections**

To prevent piracy & unlicensed games, Nintendo has made a bunch of
different PPUs: PPUs with different palettes, and PPUs with different Port
2000h/2001h/2002h.

**Third-Party Protections**

Thirty-Party games can be often DIP-switched to work with different palettes
(thus bypassing Nintendo's protection). Instead, some thirty-party games do
require some special "ID chip" mapped at 5xxxh or 5Exxh, and refuse to boot if
the thing doesn't respond with expected values (see below for details).

**Daughterboards**

These are mainly used to access more memory. But, to some level they do also
act as protection, since one needs to buy the boards. Some boards can be
DIP-switched to work with different games with ROM capacites though.

**Raid on Bungeling Bay**

This game consists of seven 8Kbyte EPROMs. Six PRG/CHR EPROMs for first CPU,
and one PRG EPROM for second CPU (without any CHR ROM here). This extra EPROM
isn't doing anything useful (no sound/video output, and coprocessor-like
maths), it's just doing a short handshake with the other CPU, and then it hangs
in an endless loop.

The purpose is unknown; it may be some crude protection (won't work if the
extra EPROM doesn't say "I am here"). Or maybe the game supports DUAL mode (and
in UNI mode, requires the second CPU to say "I am NOT here").

Basically, the game needs a response IRQ from other CPU (with don't care
response at 67E0h-67FFh), and additionally DIP 5 must be ON.

**TKO Boxing Protection (Namco)**

Protection unit is contained in a 28pin chip with part number "126 JAPAN".

```
[5E00h].Read   ;-reset data stream (returns unknown/dummy value)
[5E01h].Read   ;-return data stream (returns FFh,BFh,B7h,etc.)
```

TKO Boxing contains pre-computed 32 values in ROM:

```
FF,BF,B7,97,97,17,57,4F,6F,6B,EB,A9,B1,90,94,14    ;1st..16th read
56,4E,6F,6B,EB,A9,B1,90,D4,5C,3E,26,87,83,13,51    ;17th..32th read
```

That is, the initial value (FFh on first read) is XORed by following values:

```
40,08,20,00,80   ;XORed after 1st..5th read
40,18,20,04,80   ;XORed after 6th..10th read
42,18,21,04,80   ;XORed after 11th..15th read
42,18,21,04,80   ;etc.
42,18,21,44,88
62,18,A1,04,90
42
```

The exact way how that pattern is generated (and how it continues after 32
reads) is unknown. Note: The way how TKO Boxing is programmed, it CAN only
verify the first 31 values, and actually DOES only verify first 7 values
(unless there are further checks hidden "deeper" in the game).

**Atari RBI Baseball Protection (Namco)**

Protection unit is contained in a 28pin chip with part number "127 JAPAN".

```
[5E00h].Read   ;-reset data stream (returns unknown/dummy value)
[5E01h].Read   ;-return data stream (returns whatever...)
```

RBI Baseball verifies only two values: B4h on 5th read, and 6Fh on 10th read.
This is different as in TKO Boxing (which would return 97h and 6Bh).

**Super Xevious Protection (Namco)**

Uses whatever protection chip (unknown part number).

The game is doing the following check upon Reset:

```
Write:  [5098h]=38h, [5132h]=9Eh, [5263h]=22h, [5300h]=90h
Verify: [54FFh]=05h, [5678h]=01h, [578Fh]=89h, [5567h]=37h
Write:  [5056h]=44h, [51C8h]=72h, [526Ah]=9Ah, [5300h]=FEh
Verify: [54FFh]=05h, [5678h]=00h, [578Fh]=D1h, [5567h]=3Eh
```

That can be faked by returning following values upon [5400h..57FFh] reads:

```
05h, 01h, 89h, 37h, 05h, 00h, D1h, 3Eh
```

Unknown how the hardware works in reality; it looks like four 8bit latches, and
scrambled data, possibly by XORing data & address lines.

**Controllers**

Some VS games do reportedly some controller buttons exchanged with each other,
so they can be played only when buying a special controller front panels or so.
Unknown which games and which buttons that applies to.
