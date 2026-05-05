# Famicombox

> Source: https://problemkaputt.de/everynes.htm
> Section: Famicombox

FamicomBox 
Nintendo FamicomBox (SSS-CDS) aka Sharp FamicomStation.

Info from Kevin Horton.

FamicomBox Memory and I/O Maps

FamicomBox I/O Ports

FamicomBox Misc

FamicomBox Cartridges

FamicomBox ROM Header (at FFE0h)

FamicomBox Pinouts

FamicomBox I/O Ports

**5000h.W - Exception Trap Enable Bits (reset to 00h on power-up only)**

```
0    6.82Hz interrupt source          (0=Enable, 1=Disable)
1    Attraction Timer                 (0=Disable, 1=Enable)
2    Controller reads                 (0=Disable, 1=Enable)
3    Keyswitch rotation               (0=Disable, 1=Enable)
4    Coin insertion (and/or EXPIRED?) (0=Disable, 1=Enable)
5    Reset Button                     (0=Disable, 1=Enable)
6    Not used                         (Watchdog is always Enabled)
7    CATV connector Pin 1 detection   (0=Disable, 1=Enable)
```

**5000h.R - Exception Trap Flags**

```
0    6.82Hz interrupt source hit              (0=Trapped, 1=Normal)
1    Attraction Timer Expired                 (0=Trapped, 1=Normal)
2    Controller(s) were read (and PRESSED?)   (0=Trapped, 1=Normal)
3    Keyswitch was rotated                    (0=Trapped, 1=Normal)
4    Coin was inserted (and/or EXPIRED?)      (0=Trapped, 1=Normal)
5    Reset Button was pressed                 (0=Trapped, 1=Normal)
6    Watchdog Timer Expired (after 17.5s)     (0=Trapped, 1=Normal)
7    CATV connector Pin 1 went high/open      (0=Trapped, 1=Normal)
```

When 5000h.R is read, this register is reset, and the exception trap controller
is reset.

There seems to be a 9th exception flag in 5007h.R.Bit0. If the NINE flags are
all ones, then the reset can be considered to be caused by a "PROTECT IC" (aka
CIC, presumably) (see menu error 0Fh at 9067h).

When one of the above exceptions occurs, the CPU is reset. The menu code then
reads to see which of the sources caused the reset, and acts accordingly.

**5003h.W - Exception Trap Attraction Timer**

```
0-7  Counter value (decremented at 6.8274Hz) (255=max=37 seconds)
```

When the timer wraps from 00h to FFh it triggers an exception, if it is
enabled. (See 5000h.W and 5000h.R)

This timer is used for the "attract" mode. It lets the game run for several
seconds before it brings the menu back.

**5001h.W - Coin Chip Params & CATV Outputs (reset to 00h on power-up only)**

```
0    Coin Chip pin 1  (1=Ten Minutes per Coin)
1    Coin Chip pin 2  (1=Twenty Minutes per Coin)
2    Coin Chip pin 3  (should be always 0, except in test mode)
3    Coin Chip pin 4  (should be always 0, except in test mode)
4    Coin Chip pin 12 (should be always 0, except in test mode)
5    Coin Chip pin 14 (1=Apply Minutes?)
6    CATV Connector pin 7 (0=Free-Menu/Demo, 1=Play-and-Pay; unless TV Mode?)
7    CATV Connector pin 8 (0=High=Okay, 1=Low=Joypad/Zapper-Disconnect-Alert)
```

**5002h.W - Slot LED and RAM protect register (reset to 00h when CPU is reset)**

```
0-3  LED Select (00h=None, 01h..0Fh=Cartridge Slot LED 1..15)
4-6  RAM Write Enable (0=None, 1=0-07FFh, 2=0-0FFFh, 3=0-17FFh, 4-7=0-1FFFh)
7    LED Flash (3.414Hz) (high=flash, low=steady)
```

BIT7: err... 1=high, 0=low or what? err... flash=blinking? and steady... means
always on? err... what LED(s)... the currently selected cart-led? power-led?

**5004h.W - Slot ROM Cart control register (reset to 00h when CPU is reset)**

```
0-3  Cart Number select (00h=Menu, 01h..0Fh=Game Cart1-15)
4-5  Cart Row select    (00h=Menu, 01h=Cart1-5, 02h=Cart6-10, 03h=Cart11-15)
6    Lock (0=No change, 1=Disable Port 500xh or so; until Reset)
7    NC
```

Proper cart selection requires setting up BOTH the desired cart number
(00h-0Fh), AND the proper row.

**5005h.W - Misc Control (reset to 00h on power-up only)**

```
0    Latching Relay (1=Flip to position A) (coil on pins 1 & 10)
1    Coin Chip pin ?    ;err... which pin?  (0=Operate, 1=Config/Reset?)
2    Zapper GND (0=Disable/Low-Z; for 5007h.R connect-check, 1=Enable/GND)
3    enable 40% input of modulator (0=Disable, 1=Enable)
4    NC
5    maps to 5007h.R.Bit7     (warmboot flag or so?)  (and to 50-pin ??)
6    Joypad Enable  (0=Enable, 1=Disable)
7    Joypad Swap    (0=Swap, 1=Normal)       swapping only swaps D0 and CLK
```

err... what is this register having to do with "CATV"...? maybe the relay?

**5007h.R - Misc Status**

```
0    TV type selection (0=TV, 1=Game)   ;or... trap flag bit8... CIC, 1=trap?
1    Keyswitch turned  (0=No, 1=in middle of two positions)
2    Zapper GND (0=Enabled or Disabled+Disconnected, 1=Disabled+Connected)
3    Expansion 50-pin Edge Connector, Pin 21 (inverted)
4    CATV Connector pin 8 (0=Low, 1=High) (see also: 5001h.W.Bit7)
5    Relay Position (0=Position A, 1=Position B) (For CATV 0=Force Demo Only)
6    Expansion 50-pin Edge Connector, Pin 22 (inverted)
7    5005h.W.Bit5 (inverted)                 ;coldboot/warmboot flag?
7    err, and/or Expansion 50-pin Edge Connector, Pin 23
```

**5002h.R - Dip Switch Register**

```
0   DIP SW 1 Self Test            (0=Off=Normal, 1=On=Do continuous selftest)
1   DIP SW 2 Coin timeout period  (0=Off=10 minutes, 1=On=20 minutes)
2   DIP SW 3 Not used             (0=Off, 1=On)
3   DIP SW 4 Famicombox menu time (0=Off=7 seconds, 1=On=12 seconds)
4   DIP SW 5 Attract time, bit0 ;\(0..3 = 12,17,23,7 seconds)
5   DIP SW 6 Attract time, bit1 ;/
6   DIP SW 7 Mode, bit0 ;\(0=KEY MODE, 1=CATV MODE, 2=COIN MODE, 3=FREEPLAY)
7   DIP SW 8 Mode, bit1 ;/
N/A DIP SW 9 Feep Disable         (On=Mute the Coin feep)
N/A DIP SW 10 Controller 2 D3,D4  (Off=Disable, On=Enable) (Zapper pins)
```

Attract time: How long each game plays while in attract mode before switching
to the next.

**5003h.R - Keyswitch Position & Coin Chip Status**

```
0    Key Position 0 (0=No, 1=Yes) (??? from left) ;-(demo mode?)
1    Key Position 1 (0=No, 1=Yes) (??? from left) ;\SELECT GAME (play modes)
2    Key Position 2 (0=No, 1=Yes) (??? from left) ;/
3    Key Position 3 (0=No, 1=Yes) (??? from left) ;-SELF CHECK and GAME CHECK
4    Key Position 4 (0=No, 1=Yes) (??? from left) ;-Lockup ???
5    Key Position 6 (0=No, 1=Yes) (1st from left) ;-GAME TITLE & COUNT
6    Coin Chip Pin 9  (0=Empty/Expired, 1="Money system enabled")
7    Coin Chip Pin 10 (not used by existing software) (MAYBE expired-trap?)
```

Note: There are Black visitor keys, and red master keys. The visitor keys can
be probably turned to only 2 positions.

The naming for the key positions is very confusing. Obvious names would be "Nth
from Left" or "Bit0-5" (which may not be numbered straightly). Other naming
schemes include "ON-OFF" (on front panel, without markings on the other four
positions), "1 2 3 4 5 6" (used in the Menu cartridge), "0 1 2 3 4 6" (used by
kevtris, mabe pin-numbers, with 5=GND), and "1-OFF-ON-2-3" (seen on a japanese
webpage).

**5001h.R - Not used**

```
0-7  Not used (open bus)
```

**5004h.R - Test Connector DB-25 Inputs**

```
0-7  Input R0..R7 (DB-25 pins 2,15,3,16,4,17,5,18) (inverted; 0=High, 1=Low)
```

**5006h.W - Test Connector DB-25 Outputs (not reset, uninitialzed at power-up)**

```
0-7  Output W0..W7 (DB-25 pins 6,15,7,16,8,17,9,18)
```

**5005h.R - Expansion 50-pin Edge Connector, 8bit Input 1**

```
0-7  Input from CPU Databus; with 5005h.Read signal on Expansion Port pin 28
```

**5006h.R - Expansion 50-pin Edge Connector, 8bit Input 2**

```
0-7  Input from CPU Databus; with 5006h.Read signal on Expansion Port pin 27
```

**5007h.W - Expansion 50-pin Edge Connector, 8bit Output**

```
0-7  Output to CPU Databus; with 5007h.Write signal on Expansion Port pin 26
```

**4016h.W - Joypad Strobe**

```
0    DB-15 pin 12 and Joypad 1/2 Strobe pin 3
1    DB-15 pin 11
2    DB-15 pin 10
3-7  Not used
```

**4016h.R - Watchdog Reload, and Joypad 1**

```
0    Joypad 1 D0 pin 4                  ;Joypad 1 (or joypad 2 when swapped)
1    DB-15 pin 14
2    Expansion 50-pin Edge Connector, Pin 44 (would be Microphone in Famicom)
3    Joypad 1 D3 pin 6
4    Joypad 1 D4 pin 7
5    Expansion 50-pin Edge Connector, Pin 19
6    Expansion 50-pin Edge Connector, Pin 45
7    Expansion 50-pin Edge Connector, Pin 20
```

Reading 4016h or 4017h resets the 4bit Watchdog timer (this must be done at
least every 17.5 seconds; ie. every fifteen 0.853Hz steps).

**4017h.R - Watchdog Reload, and Joypad 2 and Zapper**

```
0    DB-15 pin 8 and Joypad 2 D0 pin 4  ;Joypad 2 (or joypad 1 when swapped)
1    DB-15 pin 7
2    DB-15 pin 6
3    DB-15 pin 5 and Joypad 2 D3 pin 6  ;Zapper Light   ;\when DIP10=Off:
4    DB-15 pin 4 and Joypad 2 D4 pin 7  ;Zapper Trigger ;/only DB-15 pins
5    Expansion 50-pin Edge Connector, Pin 17
6    Expansion 50-pin Edge Connector, Pin 43
7    Expansion 50-pin Edge Connector, Pin 18
```

Reading 4016h or 4017h resets the 4bit Watchdog timer (this must be done at
least every 17.5 seconds; ie. every fifteen 0.853Hz steps).

FamicomBox Cartridges

**Software Requirements**

For whatever reason, most or all game cartridges are containing PRG-ROM and
CHR-ROM stored on EPROMs rather than ROMs. The FamicomBox EPROMs are usually
(maybe always) containing exactly the same data as normal Famicom ROMs, without
any FamicomBox specific modifications.

However, as far as known, the Watchdog feature is active even in Game-mode, so
games MUST read the controller ports (either 4016h or 4017h) at least every
17.5 seconds; that might be a problem with a few games (with excessive intros
without controller input).

And, the games MUST contain a "header" with valid Checksums and Title (see
below). The only exception are older games (that were produced before the
FamicomBox was released; ie. before 1986/1987), for such games, the FamicomBox
menu cartridge contains a database with about 150 known checksums & titles.

FamicomBox ROM Header (at FFE0h)

**Hardware Restrictions**

According to Kevin Horton, FamicomBox games cannot use IRQs, and can contain
memory and I/O ports in the 8000h-FFFFh area only (though unknown what exactly
is causing that restrictions) (in case of the SRAM area at 6000h-7FFFh: unknown
if the games are allowed to use the internal SRAM on the FamicomBox mainboard;
instead of battery-backed SRAM used in retail game cartridges).

FamicomBox Pinouts

**72-Pin Cartridge Slots (16 slots: for menu cart and 15 game carts)**

Mostly same as 72-pin NES cartridges, see:

Cartridge Pin-Outs

Pin 16-19 are the 4bit SlotID; for use by the FamicomBox CIC chip (on NES
carts, these pins are unused "expansion" pins).

**26pin Front Panel connector (from Mainboard to Front Panel)**

```
1 - +5V
2 - +5V
3 - Front LED anode (cathode is grounded) (green LED "indicating TV/game")
4 - TV/Game Button (5V on other end, connected to pin 6)
5 - RESET Button (Low=Pressed) (used to return to Menu)
6 - +5V
7 - Coin Connector Pin 3 and connects to LED (err... what LED??)
8 - Coin Connector Pin 1 and TEST Button (Low=Add credit)
9 - Controller Pin 1 on Port 3:   (5007h.R.Bit2, 5005h.W.Bit2, Zapper GND)
10 - Controller Pin 4 on Port 1:   (4016h.R.Bit0, Joypad 1 Data)  ;\can be
11 - Controller Pin 4 on Port 2:   (4017h.R.Bit0, Joypad 2 Data)  ;/swapped
12 - Controller Pin 2 on Port 1:   (4016h.Read, Joypad 1 Clock)   ;\can be
13 - Controller Pin 2 on Port 2-3: (4017h.Read, Joypad 2 Clock)   ;/swapped
14 - Controller Pin 3 on Port 1-2: (4016h.W.Bit0, Joypad 1/2 Strobe, OUT0)
15 - Controller Pin 7 on Port 2-3: (4017h.R.Bit4, Zapper Trigger) ;\only when
16 - Controller Pin 6 on Port 2-3: (4017h.R.Bit3, Zapper Light)   ;/DIP10=On
17 - Controller Pin 6 on Port 1:   (4016h.R.Bit3)
18 - Controller Pin 7 on Port 1:   (4016h.R.Bit4)
19 - Keyswitch position 0 (Low=Selected) (5003h.R.Bit0)
20 - Keyswitch position 1 (Low=Selected) (5003h.R.Bit1)
21 - Keyswitch position 2 (Low=Selected) (5003h.R.Bit2)
22 - Keyswitch position 3 (Low=Selected) (5003h.R.Bit3)
23 - Keyswitch position 4 (Low=Selected) (5003h.R.Bit4)
24 - Keyswitch position 6 (Low=Selected) (5003h.R.Bit5)
25 - GND
26 - GND
```

**3pin Coin Mechanics connector (from Front Panel to External Coin Unit)**

```
1 - (pin8 of front panel PCB) (Low=Add credit) (also wired to TEST button)
2 - GND
3 - (pin7 of front panel PCB) "connects to LED"   err.. what LED, what for?
```

**7pin Controller Ports (3 pieces; for 2 Joypads and 1 Zapper)**

```
Pin Purpose        Port 1 (Joy1) Port 2 (Joy2)      Port 3 (Zapper)
1 - Ground         GND           GND                5005h.W.Bit2/5007h.R.Bit2
2 - Joypad Clock   4016h.R       4017h.R            4017h.R       .---------.
3 - Joypad Strobe  4016h.W.Bit0  4016h.W.Bit0       NC            | 4 3 2 1 |
4 - Joypad Data    4016h.R.Bit0  4017h.R.Bit0       NC            | 7 6 5  /
5 - Supply         +5V           +5V                +5V           '-------'
6 - Zapper Light   4016h.R.Bit3  4017h.R.Bit3/DIP10 4017h.R.Bit3/DIP10
7 - Zapper Button  4016h.R.Bit4  4017h.R.Bit4/DIP10 4017h.R.Bit4/DIP10
```

**DB-15 on the back of unit (marked "15P expand"):**

```
1 - GND                9 - 4017h.R enable
2 - audio output      10 - 4016h.W.2
3 - /IRQ              11 - 4016h.W.1
4 - 4017h.R.4         12 - 4016h.W.0
5 - 4017h.R.3         13 - 4016h.R.1
6 - 4017h.R.2         14 - 4016h.R enable
7 - 4017h.R.1         15 - +5V
8 - 4017h.R.0
```

Note: The pinout is the same as the Famicom's DB-15.

**Test Connector DB-25 on the back of the unit (marked "25P expand"):**

```
1 - +5V               14 - /IRQ
2 - 5004h.R.0         15 - 5004h.R.1
3 - 5004h.R.2         16 - 5004h.R.3
4 - 5004h.R.4         17 - 5004h.R.5
5 - 5004h.R.6         18 - 5004h.R.7
6 - 5006h.W.0         19 - 5006h.W.1
7 - 5006h.W.2         20 - 5006h.W.3
8 - 5006h.W.4         21 - 5006h.W.5
9 - 5006h.W.6         22 - 5006h.W.7
10 - GND               23 - GND
11 - GND               24 - M2
12 - GND               25 - GND
13 - GND
```

Could be used as general-purpose I/O port. Some of the Menu's selftest
functions expect the DB-25 to be strapped to the DB-15 via an external
test-adaptor (and the /IRQ pin strapped to a CATV pin?).

**8pin Screw Terminal Block (marked "CATV INTERFACE")**

```
1 - 5000h.RW.Bit7 Exception Trap (usually strapped to GND, ie. CATV pin4)
2 - Unknown
3 - Unknown
4 - GND
5 - +5V
6 - Unknown
7 - 5001h.W.Bit6                  ;Play-and-Pay (unless TV mode?)
8 - 5001h.W.Bit7 and 5007h.R.Bit4 ;Joypad/Zapper-Disconnect-Alert
```

**Expansion 50-pin Edge Connector (behind a small door on the back left side)**

```
1 - +5V                      26 - 5007h.W enable
2 - +5V                      27 - 5006h.R enable
3 - +5V                      28 - 5005h.R enable
4 - M2                       29 - +5V
5 - Audio Input              30 - +5V
6 - +5V                      31 - +5V
7 - PRG A6                   32 - PRG A7
8 - PRG A4                   33 - PRG A5
9 - PRG A2                   34 - PRG A3
10 - PRG A0                   35 - PRG A1
11 - PRG D1                   36 - PRG D0
12 - PRG D3                   37 - PRG D2
13 - PRG D5                   38 - PRG D4
14 - PRG D7                   39 - PRG D6
15 - PRG R/W                  40 - /(5000h-5FFFh)
16 - pin #1 audio             41 - /IRQ
17 - 4017h.R.5                42 - pin #2 audio
18 - 4017h.R.7                43 - 4017h.R.6
19 - 4016h.R.5                44 - 4016h.R.2 (microphone)
20 - 4016h.R.7                45 - 4016h.R.6
21 - 5007h.R.3                46 - GND
22 - 5007h.R.6                47 - GND
23 - 5007h.R.7 (+5005h.W.5 ?) 48 - GND
24 - GND                      49 - GND
25 - GND                      50 - GND
```

**3198 - 16pin Lockout Chip (CIC) (17 pieces; 1 on mainboard, 16 in carts)**

Lockout chip with additional 4bit SlotID (unlike normal NES lockout chips).

Cartridge Cicurity Chip (CIC) (Lockout Chip)

**3199 - 16pin Coin Timer/Coin Chip (1 piece; on mainboard)**

This is probably the same type of 4bit CPU which is used in CICs.

```
1   P00  5001h.W.Bit0 (unknown purpose)
2   P01  5001h.W.Bit1 (unknown purpose)
3   P02  5001h.W.Bit2 (unknown purpose)
4   P03  5001h.W.Bit3 (unknown purpose)
5   CL2  whatever MHz clock (or maybe NC)
6   CL1  whatever MHz clock (this one needed)
7   RES  whatever type of reset (maybe I/O controlled?)
8   GND  Supply GND
9   P10  5003h.R.Bit6 (reportedly "Money system enabled")
10  P11  5003h.R.Bit7 (unknown purpose)
11  P12               (unknown purpose)
12  P13  5001h.W.Bit4 (unknown purpose)
13  P20               (unknown purpose)
14  P21  5001h.W.Bit5 (unknown purpose)
15  P22               (unknown purpose)
16  VCC  Supply VCC
```

Note: Reportedly, 5005h.W.Bit1 also connects to whatever 3199 pin. Moreover,
the 3199 chip should connect to "a LED", sound feep, 40% video dimming, and
coin-insert signal.

**Modulator 7pin connector (connection from Mainboard to Modulator)**

```
Audio - Mono audio from amplifier+external+coin feep
Video - Comes from the usual transistor circuit on the PPU
B+    - Supply 5V
B+SW  - Switch (Low=Switches the game in, High=Lets TV signal pass through)
GND   - Supply Ground
40%   - Dimming (undermodulates video by 40%) (Low=Dim, High=Normal)
?     - Unknown (there are seven wires, not only six)
```

The 40% input is weird. When the time is about to expire on the coin mechanism,
it feeps and flashes the screen by using this 40% input. The LED on the coin
mech also flashes in time with the feeping.
