# Controllers

> Source: https://problemkaputt.de/everynes.htm
> Section: Controllers

Controllers
**Controller Interface**

[Controllers - I/O Ports](#controllersioports)

[Controllers - Pin-Outs](#controllerspinouts)

[Controllers - Summary of Controller Types](#controllerssummaryofcontrollertypes)

[Controllers - Summary of Controller Signals](#controllerssummaryofcontrollersignals)

[Controllers - Detection](#controllersdetection)

**Controllers**

[Controllers - Joypads](#controllersjoypads)

[Controllers - Four-Player Adaptors](#controllersfourplayeradaptors)

[Controllers - Lightguns (Zapper)](#controllerslightgunszapper)

[Controllers - Paddles](#controllerspaddles)

[Controllers - Push Buttons](#controllerspushbuttons)

[Controllers - Typewriter Keyboards](#controllerstypewriterkeyboards)

[Controllers - Piano Keyboards](#controllerspianokeyboards)

[Controllers - Keypads](#controllerskeypads)

[Controllers - Mats](#controllersmats)

[Controllers - Inflatable Controllers](#controllersinflatablecontrollers)

[Controllers - RacerMate Bicycle Training System](#controllersracermatebicycletrainingsystem)

[Controllers - Tablets](#controllerstablets)

[Controllers - Trackball and Mouse](#controllerstrackballandmouse)

[Controllers - Power Glove](#controllerspowerglove)

[Controllers - UForce](#controllersuforce)

[Controllers - Barcode Readers](#controllersbarcodereaders)

[Controllers - Pachinko](#controllerspachinko)

[Controllers - Microphones](#controllersmicrophones)

[Controllers - Reset Button](#controllersresetbutton)

[Controllers - Arcade Machines](#controllersarcademachines)

**Other Devices that connect to Controller I/O Ports**

[3D Glasses](./pictureprocessingunitppu.md#3dglasses)

[Storage Data Recorder](#storagedatarecorder)

[Storage Turbo File](#storageturbofile)

[Storage Battle Box](#storagebattlebox)

[Hori Game Repeater](#horigamerepeater)

[Headphones](#headphones)

**Another special device (not directly connected to Controller Ports)**

[R.O.B. (Robotic Operating Buddy)](#robroboticoperatingbuddy)

## Controllers - Pin-Outs

**Controller ports - NES (and newer Famicom models) - male, front side**

```
Pin Dir Player 1  Player 2  Expl./Usage                       .---------.
1   Out GND       GND       Ground                            | 4 3 2 1 |
2   Out PORT0-CLK PORT1-CLK Joystick Clock (CPU Port Read)    | 7 6 5  /
3   Out OUT0      OUT0      Joystick Serial-Start (Strobe)    '-------'
4   In  PORT0-0   PORT1-0   Joystick Serial-Data              .---------.
5   Out +5VDC     +5VDC     Supply                            | 4 3 2 1 |
6   In  PORT0-3   PORT1-3   Zapper Light, Paddle Button       | 7 6 5  /
7   In  PORT0-4   PORT1-4   Zapper Button, Paddle Position    '-------'
```

All controller inputs are inverted inside of the console, LOW arrives as "1".

Note: Older Famicom consoles do not include controller ports, instead the
joypad cables are directly attached to the console (without plugs/sockets).

**Famicom Expansion Port (standard db15, female, front side)**

Included in both older and newer Famicom consoles, not in NES consoles.

```
1  Out GND                                     .------------------------.
2  Out SOUND OUT (headphone adaptors)          | 8  7  6  5  4  3  2  1 |
3  I/O /IRQ                                     \ 15 14 13 12 11 10  9 /
4  In  port1-D4  (zapper button)                 '--------------------'
5  In  port1-D3  (zapper light)
6  In  port1-D2  (barcode battler) (turbo file data.in)
7  In  port1-D1  (joystick 4 serial input) (paddle ADC serial input)
8  In  port1-D0  (joystick 2 serial input)
9  Out port1-CLK (joystick 2+4 clock read)
10 Out OUT2      (turbo file data.clock) (tape output)
11 Out OUT1      (turbo file reset address)
12 Out OUT0      (joystick 1+2+3+4 start) (turbo file data.out)
13 In  port0-D1  (joystick 3 serial input) (paddle button) (tape input)
14 Out port0-CLK (joystick 1+3 clock read)
15 Out +5V
```

Used to connect a 3rd and 4th joystick, and various other expansion hardware.

Keep in mind that older Famicom Joypads cannot be disconnected, so the input at
Pin 8 may be disturbed by joypad 2 signals.

Note: Joypads/PowerPads/etc are normally using standard 4021 parallel-in
serial-out shift registers.

## Controllers - Summary of Controller Signals

==================== Port 4016h.Write ====================

**OUT-0 (4016h.W.Bit0)**

```
NES/Famicom Strobe Joypads, Paddle, Keyboard, etc. (load Shift-Registers)
NES Miracle Piano Data Out (and Long/Short Strobe)
NES RacerMate Bicycle Trainer - Forwarded to TX1/TX2 on 4016h/4017h.reads
Famicom Battle Box: CLK to EEPROM, and, when set, enable chipselect toggle
Famicom Doremikko Piano: Clock for next row
Famicom Turbo File Data Out
Famicom Oeka Kids Tablet Strobe (inverse of normal joypad strobe)
```

**OUT-1 (4016h.W.Bit1)**

```
Famicom 3D System (select left/right shutter for 3D Glasses)
Famicom Bandai Hyper Shot Gun: Vibration Feature Enable
Famicom Doremikko Piano: Start transfer (select 1st row)
Famicom Exciting Boxing (row select)
Famicom Keyboard Clock for Nibbles
Famicom Konami Hyper Shot Must be LOW for Player 2 "JUMP"/"RUN" Buttons
Famicom Newer-Paddle-Versions: Manually start next A/D-Conversion?
Famicom Oeka Kids Tablet Clock (manually clocked, unlike joypads)
Famicom Top-Rider Bike - Start some conversion or so?
Famicom Turbo File Reset Address to 0000h
NES Unused (not connected to Controller Port)
VS Dualsystem: Send IRQ to other CPU (0=No, 1=IRQ) (and map Shared-RAM)
```

**OUT-2 (4016h.W.Bit2)**

```
Famicom Bandai Hyper Shot Gun: Sound Feature Enable
Famicom Keyboard Tape Data Out
Famicom Konami Hyper Shot Must be LOW for Player 1 "JUMP"/"RUN" Buttons
Famicom Turbo File Data Clock
NES Unused (not connected to Controller Port)
VS Unisystem Mapper 99, Select 8K VROM Bank
```

==================== Port 4016h.Read ====================

**PORT0-0 (4016h.R.Bit0)**

```
Famicom Joypad 1 (always connected)
NES/Famicom Power Glove (per-byte strobe, and data-out after LONG strobe)
NES Hori Track (8bit joypad, 2x4bit trackball, 4bit switches/ID) (unreleased)
NES Joypad 1 (if connected)
NES Miracle Piano Data In
NES Power Glove (data-in) (Mattel)
NES RacerMate Bicycle Trainer - Get RX1 (and forward OUT0 to TX1) (Player 1)
NES Satellite & Four-Score (Joypad 1, Joypad 3, ID_A)
VS Unisystem Joypad 2 (not Joypad 1)
VS Unisystem Lightgun (5th Bit=ID=1, 7th Bit=Light, 8th Bit=Trigger)
```

**PORT0-1 (4016h.R.Bit1)**

```
Famicom Hori Track (8bit joypad, 2x4bit trackball, 4bit switches/ID)
Famicom Joypad 3 (when 4-player adaptor used)
Famicom Pachinko Controller (8bit Joypad 3 data, followed by 8bit ADC data)
Famicom Paddle 1 Button (1bit)
Famicom Keyboard Tape Data In
NES Unused (not connected to Controller Port)
```

And, probably: Famicom Power Glove (data-in) (PAX)

**PORT0-2 (4016h.R.Bit2)**

```
Famicom Microphone (built-in in Joypad 2)
NES Unused (not connected to Controller Port)
VS Unisystem Credit Service Button
```

**PORT0-3 (4016h.R.Bit3)**

```
Famicom Unused (not connected to Controller nor Expansion Port)
NES Second-Zapper Light Sensor
NES Power Pad Bits 2,1,5,9,6,10,11,7
VS Unisystem Dip Switch 1
```

**PORT0-4 (4016h.R.Bit4)**

```
Famicom Unused (not connected to Controller nor Expansion Port)
NES Second-Zapper Trigger Button
NES Power Pad Bits 4,3,12,8, and four "1"-bits
VS Unisystem Dip Switch 2
```

**PORT0-5/6 (4016h.R.Bit5/6)**

```
NES/Famicom Unused (not connected to Controller nor Expansion Port)
VS Unisystem Credit Left/Right Coin Slot
```

**PORT0-7 (4016h.R.Bit7)**

```
NES/Famicom Unused
VS Dualsystem: Master/Slave ID (0=Slave CPU, 1=Master CPU)
```

==================== Port 4017h.Read ====================

**PORT1-0 (4017h.R.Bit0)**

```
Famicom Joypad 2 (always connected) (without Start/Select)
NES Joypad 2 (if connected)
NES Satellite & Four-Score (Joypad 2, Joypad 4, ID_B)
NES RacerMate Bicycle Trainer - Get RX2 (and forward OUT0 to TX2) (Player 2)
Subor Clone: Subor Mouse Data (in conjunction with OUT-0,OUT-1,OUT-2 ?)
VS Unisystem Joypad 1 (not Joypad 2)
```

**PORT1-1 (4017h.R.Bit1)**

```
Famicom Doremikko Piano: Data fragment for current half-row
Famicom Joypad 4 (when 4-player adaptor used)
Famicom Exciting Boxing (column 1)
Famicom Keyboard Bit0 of 4-bit Nibble
Famicom Konami Hyper Shot Player 1 "RUN" Button
Famicom Mahjong Controller: Data (in conjunction with OUT-0,OUT-1,OUT-2 ?)
Famicom Paddle 1 Position (8bits)
NES Unused (not connected to Controller Port)
```

**PORT1-2 (4017h.R.Bit2)**

```
Famicom Barcode Battler (20-byte ASCII String at 1200 Baud, 8N1)
Famicom Doremikko Piano: Data fragment for current half-row
Famicom Exciting Boxing (column 2)
Famicom Keyboard Bit1 of 4-bit Nibble
Famicom Konami Hyper Shot Player 1 "JUMP" Button
Famicom Oeka Kids Tablet Ack (confirm Strobe/Clock signals)
Famicom Party Tap (Button 1, Button 4, ID "1"-Bit)
Famicom Turbo File Data In
NES Unused (not connected to Controller Port)
VS Unisystem Dip Switch 3
```

**PORT1-3 (4017h.R.Bit3)**

```
Famicom Battle Box: Dta.In (data from EEPROM)
Famicom Doremikko Piano: Data fragment for current half-row
Famicom Exciting Boxing (column 3)
Famicom Keyboard Bit2 of 4-bit Nibble
Famicom Konami Hyper Shot Player 2 "RUN" Button
Famicom Oeka Kids Tablet Data (18bits)
Famicom Paddle 2 Button (1bit)
Famicom Party Tap (Button 2, Button 5, ID "0"-Bit)
Famicom Top-Rider Bike - First 8bit shift-register
NES/Famicom Zapper Light Sensor
NES/Famicom Power Pad Bits 2,1,5,9,6,10,11,7
NES Paddle Button (1bit)
NES Power Pad Bits 2,1,5,9,6,10,11,7
VS Unisystem Dip Switch 4
```

**PORT1-4 (4017h.R.Bit4)**

```
Famicom Battle Box: Status of Dta.out (can be toggled by [4016h].reads)
Famicom Doremikko Piano: Data fragment for current half-row
Famicom Exciting Boxing (column 4)
Famicom Keyboard Bit3 of 4-bit Nibble
Famicom Konami Hyper Shot Player 2 "JUMP" Button
Famicom Paddle 2 Position (8bits)
Famicom Party Tap (Button 3, Button 6, ID "1"-Bit)
Famicom Top-Rider Bike - Second 8bit shift-register
NES/Famicom Zapper Trigger Button
NES/Famicom Power Pad Bits 4,3,12,8, and four "1"-bits
NES Power Pad Bits 4,3,12,8, and four "1"-bits
NES Paddle Position (8bits)
VS Unisystem Dip Switch 5
```

**PORT1-5/6/7 (4017h.R.Bit5/6/7)**

```
NES/Famicom Unused (not connected to Controller nor Expansion Port)
VS Unisystem Dip Switch 6/7/8
```

==================== Other Controller Port Signals ====================

**PORT0-CLK (4016h.R) and PORT1-CLK (4017h.R)**

Automatically clocked when reading 4016h and 4017h accordingly, usually
clocking serial shift registers (joypad, paddle). Other uses are:

```
Famicom Battle Box: Toggle Dta.out (always), Toggle /CS (when [4016h].W.0=1)
Famicom Doremikko Piano: Clock for 1st/2nd half of current row
FamicomBox: Watchdog Reload
NES RacerMate Bicycle Trainer - Forward OUT0 to TX0/TX1 (by 4016h/4017h.Read)
```

**/IRQ**

```
Famicom Unused (expansion port /IRQ-pin not used by any known controllers)
NES Unused (not connected to Controller Port)
```

**SOUND OUT**

```
Famicom Headphones (used by some headphone adaptors)
NES Unused (not connected to Controller Port)
```

==================== Controller Signals on Cartridge Slot ====================

**Controller I/O Ports**

```
4032h FDS Disk Status Register 1 (Disk Insert/Eject Sensor)
6000h Bandai Microphone and Buttons ?
6000h.Bit3 - Bandai Datach - Joint ROM System (Barcode Sensor)
500xh FamicomBox: Dip Switches, Keyswitch, Coin Input, Joypad/Zapper Control
```

And, video out: Used by Lightguns and R.O.B. Robot.

## Controllers - Joypads

**Joypads (or Joysticks)**

Each joypad includes an 8bit shift register, set Port 4016h/Bit0=1 to reload
the button states into the shift registers of both joypads, then reset Port
4016h/Bit0=0 to disable the shift reload (otherwise all further reads would be
stuck to the 1st bit, ie. Button A). Joypad data can be then read from bit 0 of
4016h (joypad 1) and/or bit 0 of 4017h (joypad 2) as serial bitstream of 8bit
length, ordered as follows:

```
1st         Button A         (0=High=Released, 1=Low=Pressed)
2nd         Button B         (0=High=Released, 1=Low=Pressed)
3rd         Select           (0=High=Released, 1=Low=Pressed)
4th         Start            (0=High=Released, 1=Low=Pressed)
5th         Direction Up     (0=High=Released, 1=Low=Moved)
6th         Direction Down   (0=High=Released, 1=Low=Moved)
7th         Direction Left   (0=High=Released, 1=Low=Moved)
8th         Direction Right  (0=High=Released, 1=Low=Moved)
9th and up  Unused (all 1=Low)  ;(or all 0=High when no joypad connected)
```

The console automatically sends a clock pulse to the Joypad 1 shift register
after each read from 4016h (and to joypad 2 after read from 4017h). There are
no timing restrictions, joypads can be handled as fast, or as slow, as desired.

Note that older Famicom controllers include Select & Start buttons only on
joypad 1, whilst joypad 2 probably returns unused dummy bits instead (the
values of that dummy bits are unknown, probably 0=High=Released?).

**Joypad Layout**

```
___________________________________
|    _                              |
|  _| |_                Nintendo    |
| |_   _| SELECT START              |
|   |_|    (==)  (==)  ( B ) ( A )  |
|___________________________________|
```

Jump and Run Games conventionally use A=Jump, B=Fire.

**Third-Party Joypads/Joysticks (NES)**

Third-Party NES Joypads/Joysticks can be simply plugged into normal controller
ports.

**Third-Party Joypads/Joysticks (Famicom)**

The Famicom comes with two hardwired joypads, making it difficult to use
third-party joypads or joysticks. One simple solution is to mount add-on
hardware (mechanically) on the standard joypads:

```
Climber Stick NBF-CY (Nichibutsu) (mini-dildo/nipple to be mounted on DPAD)
FamiCoin (colored "coins" to be mounted on DPAD; for better grip or so)
Super Controller (Bandai?)(joystick to be mounted on DPAD of standard joypad)
Ultech 3 Meijin-kun (joystick to be mounted on DPAD+A+B of standard joypad)
```

Another solution is to connect the devices to the 15pin expansion port (the
15pin connector lacks the joypad 1 signal, so at best, they could be shortcut
with the joypad 2 signal, or wired as joypad 3/4; aside from 3/4-player games,
many 1/2-player games also support that kind of input). Known devices are:

```
ASCII Stick L5 - one-handed joypad (not joystick) (ASCII)
Joypad with numeric keypad for Famicom Modem (connects to the 15pin port)
```

Reportedly, following further devices exist (unknown if they are mechanically
attached, or electronically connected to 15pin port, or to 7pin ports of late
Famicoms):

```
Joystick-7 and Joystick-7 Mk II
Joycard Sanusui SSS (Hudson) (with adapter for headphones)
Reggies's Joystick (with turbofire)
Super Controller II (Bandai) (with LCD screen)
Toyo Stick (Toyo)
```

**Attention:**

To support external joypads, single-player games should read joypad 3, and
treat it is alternate input for player 1. Two-player games should also support
joypad 4 (via 4-player adaptor) as alternate input for player 2.

**Crazy Climber Stick**

Crazy Climber is played with two regular joypads, but both turned by 90
degrees. Optionally, "Climber Sticks" can be mounted on the joypads, turning
the thumb-controlled DPADs into thumb-controlled joysticks.

```
_________                    _________
|    _    | Joypad 1         |    _    | Joypad 2
|  _| |_  | (Left Hand)      |  _| |_  | (Right Hand)
| |_   _| |                  | |_   _| |
|   |_|   |                  |   |_|   |
|         |                  |         |
| | SEL   |                  |         |
|         |                  | :: MIC  |
| | STA   |                  |         |
|         |                  |         |
| (B)     |                  | (B)     |
| (A)     |                  | (A)     |
|_________|                  |_________|
```

## Controllers - Lightguns (Zapper)

**Zapper (Light Gun) Ports / Connection**

Zapper state can be obtained by reading Bit3-4 of Port 4017h and/or 4016h.

Famicom Zapper connected to Famicom Expansion Port (Inputs at 4017h).

NES Zappers connected to 1st and/or 2nd Joypad Port (Inputs at 4016h, 4017h).

Most or all NES games are using 2nd Joypad Port because Famicom uses 4017h.

```
Bit3  State of the gun sight (0=High=Light detected, 1=Low=None)
Bit4  Trigger (0=High=Released, 1=Low=Pulse) (shoot on 1-to-0 transition)
```

The trigger mechanics are working as so:

```
pulled part way: soft "click" sound: 0-to-1 transition (ignored by games)
pulled full way: loud "CLANG" sound: 1-to-0 transition (games do fire)
```

One exception are Bandai games, which do accidently shoot at 0-to-1 (either
because of misunderstanding the zapper hardware, and/or for compatibility with
Bandai's own Hyper Shot gun).

The light detection flag gets set when sensing light emission from the display,
ie. when the cathode ray beam outputs a bright color (preferably white) at the
location where the gun is pointed to.

Handling the light sensor is usually done as so:

```
Output a black picture with a white-rectangle at target location
If the gun senses light, then it's a "hit".
(to reduce unnecessary flickering, do that only when trigger is pulled)
```

To avoid "hits" on other light sources, verify that there is NO light at times
when the screen should be dark (eg. shortly before end of vblank).

When sensing light, the sensor bit does reportedly stay set for 10-25 scanlines
(during hit-checks, one should check the bit around every 5 scanlines).

Note: The NES PPU doesn't have any H/V-latches for lightpen/lightgun
coordinates, so there's little chance to measure the H-position by software.
However, one may measure the vertical position by counting the time between
light and vblank (this would allow handling multiple targets at different
vertical positions within a single frame).

Note: Shooting at offscreen locations is advancing menu cursors in some games
(the games are typically also allowing to do this via joypad-select, so it
isn't stritcly necessary to support offscreen positions in emulators).

**Games compatible with the NES Zapper:**

```
Adventures of Bayou Billy (gun optional) (U) 1989 (E) 1990 Konami
Baby Boomer (unlicensed) (gun optional) (U) 1989 Jim Meuer/Color Dreams
Barker Bill's Trick Shooting (U) 1989 Nintendo
Chiller (unlicensed) (gun optional) (U) (HES) 1986 Exidy
Day Dreamin' Davey (gun optional) (U) 1990 HAL
Duck Hunt (JUE) (PC10) (VS) 1984 Nintendo
Freedom Force (U) (VS) 1988 Sunsoft
Gotcha! The Sport! (U) 1987 LJN
Gumshoe (UE) (VS) 1986 Nintendo
Gun-Nac (U) (J) 1990 Tonkin House/Tokyo Shoseki/Nexoft
Hogan's Alley (JU) (PC10) (VS) 1984 Nintendo
Laser Invasion (U) aka Gun Sight (for LaserScope or Zapper) (J) 1991 Konami
The Lone Ranger (gun optional) 1991 Konami
Mechanized Attack (gun optional) (U) 1991 SNK
Operation Wolf (gun optional) (J) (U) 1989 Taito
Shooting Range (for Hyper Shot or Zapper+Joypad) (U) 1989 Bandai
Space Shadow (for Hyper Shot or Zapper+Joypad) (J) 1989 Bandai
To The Earth (U) 1989 Nintendo
Track & Field II (in one section of the game) (U)(E) 1988/89 Konami
Wild Gunman (U) (PC10) Wairudo Ganman (J) 1984 Nintendo
```

**NES/Famicom Lightguns**

```
Nintendo Zapper (US) (old gray/dark gray version) (1985)
Nintendo Zapper (US) (new gray/orange version)
Nintendo Beam Gun (Japan) (black/brown revolver) (1985)
Nintendo VS Unisystem Lightgun (orange revolver) (2bit serial transmit)
Bandai Hyper Shot (black machine gun: zapper + joypad-like buttons) (1989)
Hyperkin FC Super Loader Gun (made by Hyperkin, NES/Zapper compatible?)
Konami LaserScope/Gun Sight (headset: headphones + voice-activated zapper)
Nexoft The Dominator: Pro Beam Light Gun (wireless lightgun)
```

As additional add-on gimmicks, Quickshot has made Sighting Scopes and Deluxe
Sighting Scopes that can be mounted on the Nintendo Zapper.

**Bandai Hyper Shot Gun (for Space Shadow) (1989)**

A black machine pistol, working (more or less) like a normal zapper combined
with an additional joypad.

```
4016h.R.Bit1 Serial 8bit joypad3-style button data
4017h.R.Bit3 Light   (0=High=Yes, 1=Low=No)
4017h.R.Bit4 Trigger (0=High=Released, 1=Low=Pressed/Held) (shoot while 1)
4016h.W.Bit1 Gun Move aka Body Vibration System (0=Off, 1=On)
4016h.W.Bit2 Sound (0=Off, 1=On)
```

The serial "joypad3" data is used as so (by Space Shadow):

```
joypad3 button B --> throw grenade
joypad3 up       --> move forward (after defeating enemy)
joypad3 select   --> toggle sound/gun-move (in title screen)
joypad3 start    --> start/pause
```

The trigger is a simple push-button without the normal zapper-mechanics, this
allows Space Shadow to support continous fire when the button is held down, but
isn't fully compatible with normal zapper games (which will fire on 1-to-0
transitions, ie. when <releasing> the Hyper Shot trigger, rather than
pressing it).

The light sensor may have different sensitivity as normal zappers (Space Shadow
does use white-rectangles, but doesn't output black-background; the
tunnel-backgrounds are fairly dark, but the backgrounds at tunnel-end are very
bright, even including some white pixels; so apparently, the hypershot gun
senses light only when aimed at all-white-pixels areas).

There seems to be some sort of rumble/vibration feature (called "Body Vibration
System" on the gun, and "Gun Move" in Space Shadow).

And, some "Sound" feature (can be enabled/disabled in Space Shadow title
screen), details are unknown; maybe causing the gun to produce sounds when
pulling the trigger, or maybe simply outputting the APU sound/music to a
speaker in the gun?

Note: Bandai's "Hyper Shot" Lightgun is not to be confused with Konami's "Hyper
Shot" Push Buttons.

**VS System Lightguns (for VS Unisystem/Dualsystem arcade machines)**

These are resembling the japanese revolver-shaped Beam Guns. The VS Unisystem
can't use the "normal" Zapper I/O lines (because it has DIP-Switches connected
to that ports), and, instead, it's injecting Lightgun data to the 8bit "Joypad"
shift-register:

```
Port.Bit      (VS-Joypad)  VS-Zapper
4016h.5th.Bit (Joy2 Up)    Connect OK   (0=Alert sound, 1=Connected)
4016h.7th.Bit (Joy2 Left)  Light Sensor (0=No,1=Light ;INVERSE of NES Zapper)
4016h.8th.Bit (Joy2 Right) Trigger      (0-to-1=Fire ;INVERSE of NES Zapper)
```

The Connect bit is used to generate an Alert sound if people are trying to
steal the gun by cutting the cable.

The gun lacks a transistor that is found in normal Zappers (so the Light sensor
will output opposite voltage).

Also mind that the VS Unisystem uses different palette(s) than NES consoles, so
the PPU color number for White (20h or 30h on NES) may differ from game to
game.

**FamicomBox Zapper**

The FamicomBox is using a standard NES-Zapper, but with some extras on the
mainboard: A somewhat bidirectional Supply GND pin on the Zapper connector
(allows to do a zapper connect check), an Disconnect-Alert signal (on a special
"CATV" connector), and a DIP-switch for disabling the Zapper. For details, see:

[FamicomBox](./famicombox.md)

**Zapper Schematic / Handheld Pistol NES004**

```
.----------.
|         1|--------------GND                           VCC -----NES.Pin5
| Sharp   2|---|

## Controllers - Push Buttons

**Party Tap (Six Players, one Button per player) (Yonezawa/Partyroom 21)**

Set of six controllers, each with one push button. Used by following games:

```
Casino Derby (J) (19xx)
Gimmi a Break - Shijou Saikyou no Quiz OuKetteisen (J) (TBS/S'PAL) (1991)
Gimmi a Break - Shijou Saikyou no Quiz OuKetteisen 2 (J) (TBS/S'PAL) (1992)
Project Q (J) (Hect/Hector) (1992)
.-----.
.----------------------------------------|1     \
|      .---------------------------------|2      |          _
|      |      .--------------------------|3      |_________| | 15pin
|      |      |      .-------------------|4      |         |_| plug
|      |      |      |      .------------|5      |
|      |      |      |      |      .-----|6     /
.-'-.  .-'-.  .-'-.  .-'-.  .-'-.  .-'-.   '-----'
|(1)|  |(2)|  |(3)|  |(4)|  |(5)|  |(6)|
'---'  '---'  '---'  '---'  '---'  '---'
```

The thing has somehow fragile timings; the different games all have different
delays before/after strobing and between reads. With max delays, it's accessed
somewhat like so:

```
wait 500 clks                                 ;-lead delay (with strobe=0)
[4016h]=01h, [4016h]=00h, wait 160 clks       ;-strobe 1-then-0 with delay
a=[4017h], wait 80 clks, b=[4017h]            ;-read 2x3bit with delay
data = (a AND 1Ch)/4 + (b AND 1Ch)*2          ;-merge (Bit0-5 = Button 1-6)
```

Moreover, at whatever time (apparently somewhere after above 1st/2nd reads), a
detection value can be read: "([4017h.R] AND 1Ch)=14h"

**Konami Hyper Shot (Two Players, two Buttons per player) (Konami)**

Set of two controllers, each with two push buttons. Used by following games:

```
Hyper Olympic (J) Konami (1985)
Hyper Sports (J) Konami (1985)
```

Accessed as shown below.

```
4016h.W.Bit1  Select Player 2 "JUMP"/"RUN" Buttons (0=Low=Yes) ;\usually, set
4016h.W.Bit2  Select Player 1 "JUMP"/"RUN" Buttons (0=Low=Yes) ;/both to 0
4017h.R.Bit1  Player 1 "RUN" Button  (0=High=No, 1=Low=Pressed and OUT-2=LOW)
4017h.R.Bit2  Player 1 "JUMP" Button (0=High=No, 1=Low=Pressed and OUT-2=LOW)
4017h.R.Bit3  Player 2 "RUN" Button  (0=High=No, 1=Low=Pressed and OUT-1=LOW)
4017h.R.Bit4  Player 2 "JUMP" Button (0=High=No, 1=Low=Pressed and OUT-1=LOW)
```

Note: For whatever reason, the buttons are wired to OUT-2/OUT-1 (rather than
being wired to GND), this would allow to select/deselect the buttons; in the
existing software both OUT-2 and OUT-1 are always set to LOW.

```
.--------------------------.     .--------------------------.
|          imanoK          |     |          imanoK          |
|         tohSrepyH        |     |         tohSrepyH        |
|    PMUJ           NUR    |     |    PMUJ           NUR    |
_|    ( )      I     ( )    |    _|    ( )     II     ( )    |
/ |    JUMP           RUN    |   / |    JUMP           RUN    |
|  |         HyperShot        |  |  |         HyperShot        |
|  |          Konami          |  |  |          Konami          |
|  '--------------------------'   \ '--------------------------'     _
\                                 '--------------------------------| | 15pin
'-----------------------------------------------------------------|_| plug
```

The pads don't explicitly have "front/back" sides (and can be turned either way
around); however, for shooting left/right in Hyper Sports they should be turned
this way: JUMP=Left, RUN=Right.

Theoretically, one could as well use normal joypad buttons, however, Konami has
intentionally crippled them to be working ONLY with their Hyper Shot: The games
are actually reading normal joypad data, but are then erasing all joypad bits
(except start/select), and are then replacing them by the Hyper Shot Button
bits.

Note: Konami's "Hyper Shot" Buttons are not to be confused with Bandai's "Hyper
Shot" Lightgun.

## Controllers - Piano Keyboards

**Doremikko (Three octaves; aka two & two-half octaves) (Konami)**

Piano keyboard for the "Doremikko" FDS (Famicom Disk System) game.

```
_____________________________________________
|  _________________________________________  |
| | U U U | U U | U U U | U U | U U U | U U | |
| | U U U | U U | U U U | U U | U U U | U U | | Keys
| | | | | | | | | | | | | | | | | | | | | | | |
|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|_|
F G A H C D E F G A H C D E F G A H C D E    Notes
|------->X    Octaves
36..47         48..59    |   60..71        72..83    84   Key Numbers
|
Middle C
49 Piano Keys (29 White Keys, 20 Black Keys) (4 octaves, plus next higher C)
1  Foot Pedal (Sustain)
8  Push Buttons (Mode/Volume Selection)
```

The thing can be connected to NES, SNES, and other consoles/computers.

Controllers - Piano - Miracle Piano Controller Port

Controllers - Piano - Miracle Piano MIDI Commands

Controllers - Piano - Miracle Piano Instruments

Controllers - Piano - Miracle Pinouts and Component List

## Controllers - Piano - Miracle Piano MIDI Commands

The Miracle is always using MIDI messages (no matter if the messages are
transferred through MIDI or RS232 or NES/SNES/Genesis controller cables). Below
lists the supported MIDI messages (including "Undocumented" messages, which are
used by the Miracle's SNES software, although they aren't mentioned in the
Miracle's Owner's Manual).

**MIDI Information Sent FROM/TO The Miracle keyboard**

```
Expl.                     Dir  Hex
Note off (Undocumented)     W  8#h,,00h   ;same as Note ON with velo=00h
Note on/off command       R/W  9#h,,
Main volume level           W  B0h,07h,
Sustain on/off command    R/W  B#h,40h,
Local control on/off        W  B0h,7Ah,
All notes off               W  B#h,7Bh,00h
Patch change command (*)  R ?? C#h,     ;TO keyboard = Undocumented
Miracle button action     R    F0h,00h,00h,42h,01h,01h,,F7h
Unknown (Undocumented)      W  F0h,00h,00h,42h,01h,02h,,F7h   ;???
Keyboard buffer overflow  R    F0h,00h,00h,42h,01h,03h,01h,F7h
Midi buffer overflow      R    F0h,00h,00h,42h,01h,03h,02h,F7h
Firmware version request    W  F0h,00h,00h,42h,01h,04h,F7h
Miracle firmware version  R    F0h,00h,00h,42h,01h,05h,,,F7h
Patch split command         W  F0h,00h,00h,42h,01h,06h,0#h,,,F7h
Unknown (Undocumented)      W  F0h,00h,00h,42h,01h,07h,F7h        ;???
All LEDs on command         W  F0h,00h,00h,42h,01h,08h,F7h
LEDs to normal command      W  F0h,00h,00h,42h,01h,09h,F7h
Reset (Undocumented)        W  FFh
```

Direction: R=From keyboard, W=To keyboard

Notes: (*) Patch change FROM Keyboard is sent only in Library mode.

```
N#h         Hex-code with #=channel (#=0 from keyb, #=0..7 to keyb)
Key (FROM Miracle: 24h..54h) (TO Miracle: 18h..54h/55h?)
Velocity (01h..7Fh, or 00h=Off)
Volume (00h=Lowest, 7Fh=Full)
Flag (00h=Off, 7Fh=On)
Instrument (00h..7Fh) for all notes
Instrument (00h..7Fh) for notes 24?/36-59, lower patch number
Instrument (00h..7Fh) for notes 60-83/84?, upper patch number
. Version (from version 1.0 to 99.99)
button on/off (bit0-2:button number, bit3:1=on, bit4-7:zero)
```

Data from piano is always sent on first channel (#=0). Sending data to piano
can be done on first 8 channels (#=0..7), different instruments can be assigned
to each channel. Although undocumented, the SNES software does initialize 16
channels (#=0..0Fh), unknown if the hardware does support/ignore those extra
channels (from the instrument table: it sounds as if one could use 16
single-voice channels or 8 dual-voice channels).

## Controllers - Piano - Miracle Pinouts and Component List

**25pin SUBD connector (J6)**

```
1  PC/Amiga/Mac RS232 GND (also wired to RTS)
2  PC/Amiga/Mac RS232 RxD
3  PC/Amiga/Mac RS232 TxD
7  NES/SNES/Genesis GND
10 NES/SNES/Genesis Data
13 NES/SNES/Genesis Strobe
14 Sense SENSE0 (0=MIDI Output off, 1=MIDI Output on)
15 Sense SENSE1 (0=9600 Baud; for RS232, 1=31250 Baud; for MIDI)
19 NES/SNES/Genesis Clock
all other pins = not connected
```

For PC/Mac RS232 wire SENSE0=GND, SENSE1=GND

**Miracle NES and SNES Cartridges**

According to the ROM Headers: The SNES cartridge contains 512Kbyte Slow/LoROM,
and no SRAM (nor other storage memory). The NES cartridge contains MMC1 mapper,
256Kbyte PRG-ROM, 64Kbyte CHR-ROM, and no SRAM (nor other storage memory).

**Miracle Piano Component List (Main=Mainboard Section, Snd=Sound Engine)**

```
U1   Snd  16pin TDA7053 (stereo amplifier for internal speakers)
U2   Snd   8pin NE5532 (dual operational amplifier)
U3   Snd  16pin LM13700 or LM13600 (unclear in schematic) (dual amplifier)
U4   Snd  14pin LM324 (quad audio amplifier)
U5   Main  3pin LM78L05 (converts +10V to VLED, supply for 16 LEDs)
U6   Main 14pin 74LS164 serial-in, parallel-out (to 8 LEDs)
U7   Main 14pin 74LS164 serial-in, parallel-out (to another 8 LEDs)
U8   Main  5pin LM2931CT (converts +12V to +10V, and supply for Power LED)
U9   Main  3pin LM78L05 (converts +10V to +5REF)
U10  Snd  14pin TL084 (JFET quad operational amplifier)
U11  Snd  40pin J004 (sound chip, D/A converter with ROM address generator)
U12  Snd  32pin S631001-200 (128Kx8, Sound ROM for D/A conversion)
U13  Main  3pin LM78L05 (converts +10V to VCC, supply for CPU and logic)
U14  Main 40pin AS0012 (ASIC) Keyboard Interface Chip (with A/D for velocity)
U15  Main 40pin 8032 (8051-compatible CPU) (with Y1=12MHz)
U16  Snd  40pin AS0013 (ASIC)
U17  Main 28pin 27C256 EPROM 32Kx8 (Firmware for CPU)
U18  Main 28pin 6264 SRAM 8Kx8 (Work RAM for CPU)
U19  Main 16pin LT1081 Driver for RS232 voltages
U20  Main  8pin 6N138 opto-coupler for MIDI IN signal
S1-8 Main  2pin Push Buttons
S9   Main  3pin Power Switch (12V/AC)
J1   Main  3pin 12V AC Input (1 Ampere)
J2   Main  2pin Sustain Pedal Connector (polarity is don't care)
J3   Snd   2pin RCA Jack Right
J4   Snd   2pin RCA Jack Left
J5   Snd   5pin Headphone jack with stereo switch (mutes internal speakers)
J6   Main 25pin DB25 connector (RS232 and SNES/NES/Genesis controller port)
J7   Main  5pin MIDI Out (DIN)
J8   Main  5pin MIDI In (DIN)
JP1  Main 16pin Keyboard socket right connector
JP2  Main 16pin Keyboard socket left connector
JP3  Snd   4pin Internal stereo speakers connector
```

Note: The official original schematics are released & can be found in
internet.

## Controllers - Mats

**Power Pad (Dance Mat) (Bandai/Nintendo)**

The Power Pad aka Family Trainer aka Family Fun Fitness is a device made by
Bandai/Nintendo (1987/1988) which serves as an "exercising fun center" for the
whole family. That is, a large (1x1 meters) vinyl mat with 12 touch-sensitive
areas, or actually step-sensitive areas, since it's intended to be put on the
floor. The mat has two sides, with different patterns drawn on each side.
Supported games are:

```
Athletic World (J) (U) (E) 1986-1988
Class Track Meet/Running Stadium/Stadium Events (J) (U) (E) 1986-1988
Aerobics Studio/Dance Aerobics (J) (U) 1987/1989
Fuuun! Takeshi Shiro 2 (J) 1988
Jogging Race (J) 1987
Meiro Daisakusen (J) 1987
Power Pad Test Program by Tennessee Carmel-Veilleux (PD) (UE)
Rai Rai! Kyonshiizu: Baby Kyonshi no Amida Daiboken (J) 1989
Short Order / Eggsplode! (U) 1989
Street Cop/Manhattan Police (J) (U) 1987/1989
Super Team Games/Daiundoukai (J) (U) 1987/1988
Totsugeki! Fuuun Takeshi Shiro (J) 1987
```

**Tap-tap Mat (igs) (Japan only)**

Another mat, this one is smaller than the Power Pad mat, and one is supposed to
hit the fields with a plastic hammer. Used by only one known game:

```
Super Mogura Tataki!! - Pokkun Moguraa (J) (bundled with mat & hammer)
```

**NES Version I/O Access (outside Japan) (Bandai/Nintendo Mats)**

Connects to 7pin controller port (typically the mat connects to port 2, and a
normal joypad for menu selections to port 1). To read the button states:

```
Output 1-then-0 to Bit0 of Port 4016h
Read eight times from Port 4017h (or 4016h, when plugged into port 1)
```

Each read receives two button states in Bit 3 and 4, in following order:

```
Bit4  4,3,12,8,u,u ,u ,u (0=High=Released, 1=Low=Pressed) (u=Unused always 1)
Bit3  2,1,5 ,9,6,10,11,7 (0=High=Released, 1=Low=Pressed)
```

Whereas, "1-12" are button numbers as drawn on Side B, or equivalent buttons on
Side A (horizontally mirrored, of course).

**FAMICOM Version I/O Access (Japan) (Bandai/Nintendo Mats and igs Mats)**

Connects to 15pin expansion port. Unlike in NES version, the buttons are read
via a simple row/column matrix (without shift-register). The overall I/O access
is same for Nintendo/Bandai-mats and igs-mats:

```
4016h.W.Bit0 Select Lower row  (9..12) (0=Low=Select, 1=High=No)
4016h.W.Bit1 Select Middle row (5..8)  (0=Low=Select, 1=High=No)
4016h.W.Bit2 Select Upper row  (1..4)  (0=Low=Select, 1=High=No)
4017h.R.Bit1 Read Right-most column   (4,8,12) (0=High, 1=Low=Pressed)
4017h.R.Bit2 Read Right-middle column (3,7,11) (0=High, 1=Low=Pressed)
4017h.R.Bit3 Read Left-middle column  (2,6,10) (0=High, 1=Low=Pressed)
4017h.R.Bit4 Read Left-most column    (1,5,9)  (0=High, 1=Low=Pressed)
```

After selecting a Row one should execute some delay before reading Column data:
For the Nintendo/Bandai-mats this should be a huge 1800 cycle delay. The
igs-mat can be used with shorter 138 cycle delays, but results might be
unstable (the tap-tap game reads all data twice, and retries reading until both
results are same).

**Power Pad Layout (Side A and Side B)**

Side A has 2 red fields, 6 blue fields, and 4 hidden fields.

Side B has 12 fields, numbered 1..12, the left fields (1,2,5,6,9,10) are blue,
the other are red.

```
____________.-------.____________     ____________.-------.____________
|POWER PAD   '======='      SIDE B|   |POWER PAD   '======='      SIDE A|
|                                 |   |                                 |
|  .---.   .---.   .---.   .---.  |   |          .---.   .---.          |
|-(  1  )-(  2  )-(  3  )-(  4  )-|   |         (     ) (     )         |
|  '---'   '---'   '---'   '---'  |   |          '---'   '---'          |
|---------------------------------|   |                                 |
|  .---.   .---.   .---.   .---.  |   |  .---.   .---.   .---.   .---.  |
|-(  5  )-(  6  )-(  7  )-(  8  )-|   | (     ) (     ) (     ) (     ) |
|  '---'   '---'   '---'   '---'  |   |  '---'   '---'   '---'   '---'  |
|---------------------------------|   |                                 |
|  .---.   .---.   .---.   .---.  |   |          .---.   .---.          |
|-(  9  )-( 10  )-( 11  )-( 12  )-|   |         (     ) (     )         |
|  '---'   '---'   '---'   '---'  |   |          '---'   '---'          |
|_________________________________|   |_________________________________|
_________.-------------._________
|         |// Tap-tap   |  igs    |
|         |/    Mat     |         |
|---------'============='---------|
|  .---.   .---.   .---.   .---.  |

## Controllers - RacerMate Bicycle Training System

CompuTrainer by RacerMate. Used only by RacerMate Challenge II.

```
4016h.W.Bit0  OUT0 (to be forwarded to TX1/TX2 on 4016h/4017h.reads)
4016h.R.Bit0  Get RX1 (and forward OUT0 to TX1) (Player 1)
4017h.R.Bit0  Get RX2 (and forward OUT0 to TX2) (Player 2) (if any)
```

The transfer rate is generated by an IRQ timer in the cartridge (the timer's
baudrate is unknown; the transfers do require at least 27 IRQs per 60Hz frame,
so the IRQ rate must be at least 1620 Hz or higher). In practice, 1024 clks per
IRQ seems to be too slow (video glitches), 512 clks too fast (1/10 second
counter increases non-linear), so, the IRQ rate is probably something around
768 clks.

**Transfer Packets (per player) (48 TX Bits & 48 RX Bits)**

```
First Frame:
2 IRQs   Leading Pause (TX levels left unchanged)
1 IRQ    Output 1st TX Bit (Start Bit "0")   Input nothing
23 IRQs  Output 2nd..24th TX Bit             Input 1st..23rd RX Bit
1 IRQ    Output nothing (keep 24th TX bit)   Input 24th RX Bit
0 IRQs   Ending pause (IRQs should be disabled until next Vblank NMI)
Second Frame:
2 IRQs   Leading Pause (TX levels left unchanged)
1 IRQ    Output 25th TX Bit (Start Bit "1")  Input nothing
23 IRQs  Output 26nd..48th TX Bit            Input 25th..47rd RX Bit
1 IRQ    Output nothing (keep 48th TX bit)   Input 48th RX Bit
0 IRQs   Ending pause (IRQs should be disabled until next Vblank NMI)
```

Both the 48bit-RX and 48bit-TX streams are interleaved:

```
24 "odd" bits  (1st, 3rd, 5th, 7th, ... 47th bits)
24 "even" bits (2nd, 4th, 6th, 8th, ... 48th bits)
```

**Odd TX Bits (1st, 3rd, 5th, 7th, ... 47th TX bits) (24bit from NES to Bike)**

```
1bit  Must be "0" (start bit of packet-half transferred in First Frame)
6bit  Unknown (seems to be fixed: 1,1,0,1,0,0)   ;(aka 2Ch/4, LSB first)
5bit  Data, Bit0..4  (LSB first)
1bit  Must be "1" (start bit of packet-half transferred in Second Frame)
7bit  Data, Bit5..11 (LSB first)
4bit  Index, Bit0..3 (LSB first)
```

The Index/Data Pairs are:

```
Index=1, Grade (signed)
Index=2, Wind (signed)
Index=3, Weight (LBS)
Index=4, Pulse Target Min (BPM+800h) ;\player 1 only
Index=5, Pulse Target Max (BPM+800h) ;/
Index=5, Zero                        ;-player 2 only
Index=F, Start Race (Data=001h)
```

Unknown if there are further ones... eg. to stop/pause/resume race?

**Even RX Bits (2nd, 4th, 6th, 8th, ... 48th RX bits) (24bit from Bike to NES)**

```
1st EvenRxByte --> Button flags (bit0-5) SpecialStart (bit6)
2nd EvenRxByte --> 8bit Data Fragment
3rd EvenRxByte --> 4bit Data Fragment (LSBs), 4bit Index (01h..0Eh) (MSBs)
```

The 12bit data fragments are stored in an array (with index 01h..0Eh), there
are thus 14x12bit = 168 bits in that array. There are two separate arrays (one
per bike).

```
XXX content of the array is unknown (presumably speed and such things?)
```

The Button bits are:

```
0  Button RESET                  (0=High=Released, 1=Low=Pressed)
1  Button F1 (START/PAUSE/LEAVE) (0=High=Released, 1=Low=Pressed)
2  Button F3 (SET/SELECT)        (0=High=Released, 1=Low=Pressed)
3  Button +  (PLUS/UP)           (0=High=Released, 1=Low=Pressed)
4  Button F2 (DISPLAY)           (0=High=Released, 1=Low=Pressed)
5  Button -  (MINUS/DOWN)        (0=High=Released, 1=Low=Pressed)
6  Start of Special Odd RX Bits  (?)
7  Unknown/unused?               (?)
```

The Index/Data Pairs are:

```
Index=0     Unknown/Ignored
Index=1     Speed 12bit (0..FFFh = 0..81.9 MPH)  (ie. 1/50 MPH units)
Index=2     Watts 12bit (0..FFFh = 0..3054 Watts) (roughly 3/4 Watt units)
Index=3     Pulse 8bit (0..FFh = 0..255 BPM) (and MSBs = 4bit Flags)
Index=4..E  Unknown/Stored in memory (but seems to have no effect)
Index=F     Unknown/Ignored
Unknown if/how/where RPM is transferred.
Speed (MPH) also affects distance (DST).
```

The 4bit flag-field for the Pulse data is:

```
Bit8  Blink Heart-Symbol
Bit9  Show "E"
Bit10 Show "LO"  ;\when both set: treated as "sensor not connected"
Bit11 Show "HI"  ;/(ie. showing "--" instead of the pulse value)
```

**Even TX Bits (2nd, 4th, 6th, 8th, ... 48th TX bits) (24bit from NES to Bike)**

These bits are simply inverse bits that are preceeding the following Odd TX
Bits. Ie. a "0" bit will be transferred as "1-then-0" bit-pair, and "1" bit as
"0-then-1". The receiver (control panel) may use that transitions for
synchronization (eg. in case of baudrate inaccuracies).

In case of the 24th/48th TX bits, this "wraps" to next frame, ie. they are
inverse of the of the start-bit of the next frame.

**Odd RX Bits (1st, 3rd, 5th, 7th, ... 47th RX bits) (24bit from Bike to NES)**

These bits can be inverse data bits that are preceeding the normal Even RX Bits
(similar as for TX).

Alternately, the odd RX bits can contain special information (probably an
extension of the original transfer protocol). Presence of that special info is
determined as so:

```
(EvenRxBytes <> (OddRxBytes XOR FFFFFFh)) AND Parity(1stOddRxByte)=odd
```

If that condition is true, then the 3 odd bytes are a fragment of a 24-byte
special array; it does thus take 8 packets to transfer the whole array; the
array-start is indicated by bit6 of the "button" byte.

```
XXX content of the array is unknown ...
XXX the software seems to manage only ONE array (not 2 arrays for 2 bikes)
```

**Control Panel**

The control panel connects to both controller ports, which leaves no room for
connecting joypads or other controllers. So, menu selections are solely
controlled by the 6 buttons on the control panel. The only other inputs are the
rear-wheels rotation speed, and the pulse sensor.

```
.-------------------------.
|      CompuTrainer       |
| .---------------------. |
| |         LCD         | |

## Controllers - Trackball and Mouse

**Hori Track**

```
Moero Pro Soccer (J) 1988 Jaleco
Operation Wolf (J) (U) 1989 Taito (also supports Zapper lightgun)
Putt Putt Golf (FDS) 1989 Pack-In-Video
US Championship V'Ball (J) 1989 Technos Japan Corp
```

Hori seems to have planned both NES and Famicom versions of the controller; as
far as known, the NES version wasn't released, but, most of the existing games
(except Putt Putt Golf) do contain both NES and Famicom controller protocol
versions:

```
Version   Input Data from      ID Bits   Connector   Released
Famicom   [4016h/4017h].Bit1   0,1       15pin       Yes
NES       [4016h/4017h].Bit0   1,0       7pin        No
```

Controller Bits:

```
(to be preceeded by normal joypad like 1-then-0 strobing on OUT-0)
1st..8th bit   --> Same as normal joypad data
9th..12th bit  --> Axis 1, signed 4bit (MSB first, inverted, 1=Low=Zero)
13th..16th bit --> Axis 2, signed 4bit (MSB first, inverted, 1=Low=Zero)
17th           --> L/R mode switch (0=High=R-Mode, 1=Low=L-Mode)
18th           --> Unknown/unused (probably SPEED LO/HI switch) (=?)
19th           --> ID Bit1 (0=High=Famicom, 1=Low=NES)
20th           --> ID Bit0 (0=High=NES, 1=Low=Famicom)
21th..24th     --> Unknown/unused (read by software, but seems to be unused)
25th and up    --> Unknown/unused (probably whatever padding bits)
```

Trackball Orientation:

```
When 17th Bit=1:  (L-Mode) (supported by all games)
Axis 1 is to be treated as Y-axis (POSITIVE = DOWN = towards DPAD)
Axis 2 is to be treated as X-axis (POSITIVE = RIGHT = towards START/SELECT)
When 17th Bit=0:  (R-Mode) (not supported by Operation Wolf)
Axis 1 is to be treated as X-axis (POSITIVE = LEFT = towards DPAD)
Axis 2 is to be treated as Y-axis (POSITIVE = DOWN = towards START/SELECT)
```

DPAD orientation is unknown (according to manual, it sounds like UP=Towards
Ball, RIGHT=Towards B-Button) (in software, this should be probably always kept
handled as so, regardless of the L/R switch).

```
___________       L-Mode         R-Mode       ___________
|    ___    |\ __________         __________ /|    ___    |
|  .'   '.  | |          |\     /|    _     | |  .'   '.  |  HORI TRACK
| | BALL  | | |  STA     | |   | |  _| |_   | | | BALL  | |
| |       | | |  // SEL  | |   | | |_   _|  | | |       | |  HORI ELECTRIC
|  '.___.'  | |     //   | |   | |   |_|    | |  '.___.'  |  CO.LTD.
|___________| |        A | |   | |          | |___________|  MODEL TRK-7
|            \|    .''.  | |   | |  .''.    |/            |  MADE IN JAPAN
|_____________|   |    | | |   | | |    |   |_____________|
|     _          B '..'  | |   | |  '..' B                |  Note:
|   _| |_    .''.        | |   | |        .''.       STA  |  L/R switch and
|  |_   _|  |    |       | |   | |       |    |   SEL \\  |  SPEED LO/HI
|    |_|     '..'        | |   | |        '..' A   \\     |  switch are at
|________________________| |   | |________________________|  bottom-side
| H O R I  T R A C K     \ |   | /                        |
|_________________________\|   |/_________________________|
```

**Joyball Note**

There's also a roughly similar looking controller called Joyball. However, that
thing is just a regular joystick with "ball-shaped" handle, not a trackball.

**Subor Mouse**

The Subor Mouse is bundled with a keyboard-shaped NES clone; the mouse is
supported by only one known cartridge (bundled with the mouse & NES clone):

```
Educational Computer 2000 (RU) (with mimmicked russian Win95 GUI)
```

Reading a mouse byte is done as so:

```
[4016h]=01h
wait 28 clks (14 NOPs)
[4016h]=06h
read 8bits from [4017h].R.Bit0 (MSB first) (bits are NOT inverted!)
```

Mouse response can be a single-byte (max +/-1 mickey), or 3-byte (max +/-31
mickeys). The response type & index is indicated in lower 2bit of the
bytes.

Single-Byte Packet:

```
7     Left Button     (1=Pressed)
6     Right Button    (1=Pressed)
5-4   Step X (0=None, 1=Plus 1/Right, 2=Treated same as 1, 3=Minus 1/Left)
3-2   Step Y (0=None, 1=Plus 1/Down,  2=Treated same as 1, 3=Minus 1/Up)
1-0   Must be 0 for single-byte packet
```

1st-byte of Three-byte Packet:

```
7     Left Button     (1=Pressed)
6     Right Button    (1=Pressed)
5     Direction X (0=Plus/Right, 1=Minus/Left)
4     Unsigned Step X, Bit4
3     Direction Y (0=Plus/Down, 1=Minus/Up)
2     Unsigned Step Y, Bit4
1-0   Must be 1 for 1st byte of multi-byte packet
```

2nd-byte of Three-byte Packet:

```
7-6   Unused (maybe copy of buttons)
5-2   Unsigned Step X, Bit3-0
1-0   Must be 2 for 2nd byte of multi-byte packet
```

3rd-byte of Three-byte Packet:

```
7-6   Unused (maybe copy of buttons)
5-2   Unsigned Step Y, Bit3-0
1-0   Must be 3 for 3rd byte of multi-byte packet
```

Threshold must be implemented by software (as done in Educational Computer
2000):

```
00h..0Eh Mickeys ---> Mul 1.0 --> 00h..0Eh Pixels
0Fh..13h Mickeys ---> Mul 1.5 --> 16h..1Ch Pixels
14h..18h Mickeys ---> Mul 2.0 --> 28h..30h Pixels
19h..1Dh Mickeys ---> Mul 2.5 --> 3Eh..48h Pixels
1Eh..1Fh Mickeys ---> Mul 3.5 --> 69h..6Ch Pixels
```

Motion counters are reset to zero after reading. Data should be read per NMI
(read at least one byte, and, in case of 3-byte response: read all 3 bytes).

Caution:

The Subor Clone does also contain a keyboard (and joypads), reading the
controllers might somehow interfere. Namely, Subor is doing the keyboard
reading AFTER and ONLY AFTER single-byte-mouse reads (in case of
multi-byte-mouse reads, it's completely omitting keyboard reading in that
frame) (though unknown if that kind of handling is really required).

Controllers - Typewriter Keyboards

## Controllers - Power Glove Transmission Protocol (RX/TX)

**Transmit (TX) (Configuration)**

Transmission mode is entered by outputting a LONG strobe signal:

```
Set [4016h].W.Bit0=1, then wait around 3330 clks      ;-Enter TX Mode
```

Thereafer, the TX packet can be sent bit-by-bit:

```
Set [4016h].W.Bit0=Databit                            ;-Output Data Bit
Do dummy-read from [4016h].R                          ;-Output Clk Pulse
```

Bytes are sent MSB first. Insert a small delay between each two bytes:

```
1280 clks should be fine; as used by Super Glove Ball ;-Delay between bytes
(the older Bad Street Brawler game used 2304 clks)
```

For the standard Analog mode with 9-byte response, the transmitted packet
should be usually following 7 bytes: 06h,C1h,08h,00h,02h,FFh,01h. For details
on other TX packets, see:

Controllers - Power Glove TX Packets (Configuration Opcodes)

After the transfer, the value of the lastmost databit may be left on the strobe
line; the glove doesn't seem to treat this as new LONG strobe.

**Receive (RX) (Read Sensors/Buttons)**

Read one Status Byte per frame (ie. via 60Hz NMI handler), just like normal
joypad reading:

```
Set [4016h].W.Bit0 to 1-then-0                        ;-Strobe
Read 8bits from [4016h].R.Bit0 (MSB first, inverted)  ;-Read Byte
```

This byte indicates if the glove is ready to send a new data packet:

```
5Fh (received) aka A0h (when XORing by FFh)   --> analog mode, ready
00h? (received) aka FFh? (when XORing by FFh) --> analog mode, not ready (?)
other (joypad data)                           --> joypad emulation mode
```

If the Status Byte indicates Ready, then the next some bytes (usually 9 bytes)
are packet data, these can be read as so:

```
wait 100 clks                                         ;-Delay between bytes
Set [4016h].W.Bit0 to 1-then-0                        ;-Strobe (on each byte)
Read 8bits from [4016h].R.Bit0 (MSB first, inverted)  ;-Read Byte
XOR byte by FFh                                       ;-Undo inversion
```

For details on the packet content, see:

Controllers - Power Glove RX Packets (Position/Sensor Data)

After the transfer, a long pause may be required (it seems as if the glove
terminates RX mode via a timeout); best wait until next 60Hz frame before
trying to receive a new packet.

The conversion time isn't constant (for example, flex A/D conversion time seems
to increase when making a tight fist). Normally, status "Ready" should be
received around every 3 frames. Generate a timeout if ready isn't seen after 20
frames; that might happen if the glove isn't connected, or not initialized (not
in analog mode), or if the user has hit the PROG button.

**Caution**

Reading [4016h].R.Bit0 does, of course, work on NES ONLY. Unknown how to read
data from the japanese PAX Power Glove; [4016h].R.Bit1 should be a good guess.

The existing games expect the glove connected to port 1 (4016h.R), alternately
it should also work in port 2 (4017h.R). When trying to connect two gloves, the
ultrasonic speakers would disturb each other, so only Flex and Buttons would
work.

## Controllers - Power Glove RX Packets (Position/Sensor Data)

**Packet Summary**

The standard packets are 9 bytes in size (not counting the preceeding "ready"
byte; and obviously not counting the preceeding/following "not ready" bytes).

```
1st byte: Signed X-Coordinate
2nd byte: Signed Y-Coordinate
3rd byte: Signed Z-Coordinate
4th byte: Wrist Rotation Angle (around Z-axis)
5th byte: Finger Flex Sensors
6th byte: Control Pad Buttons
7th byte: Unknown/unused (00h)
8th byte: Unknown/unused (00h)
9th byte: Error Flags
```

Note: It is possible to disable some of the response bytes (via Mask Word in
configuration packet). For example, one may want to disable 7th..8th byte
(since they seem to be useless, and waste around 500 clks RX time); when doing
so, the numbering of the following bytes changes (ie. in that example, Error
Flags in "9th" byte would move to location of "7th" byte).

**1st/2nd/3rd byte: Signed X/Y/Z-Coordinates**

```
1st byte: X-coordinate (-80h=Left, +7Fh=Right)
2nd byte: Y-coordinate (-80h=Down, +7Fh=Up)
3rd byte: Z-coordinate (-80h=Forward, +7Fh=Back)  ;Forward=Towards Screen
```

All coordinates are relative to the "Center" (the location where one has most
recently pushed the Center button).

Warning: There's a software bug in the Super Glove Ball game: When moving the
glove too fast, the sprite jumps to random locations in left screen half,
and/or stays stuck at botton-most screen coordinate. As a workaround, emulators
may limit the distance to max 31 units per packet.

UNKNOWN if the glove is calibrating itself to biggest/smallest coordinates it
has seen (similar as for the finger flex calibration).

If so, the calibrated range would "grow" each time when moving towards the
boundaries; to avoid that "growing" effect, it may be a good idea to clip
values to -40h..3Fh in software (that, being the "used" range, and values
beyond that range being a "dead" zone) (if the user gets into the dead zone,
nothing bad happens as it's still far to the -80h/+7Fh maximum boundaries where
the growing effect might take place).

**4th byte: Wrist Rotation Angle (around Z-axis)**

```
7-4  Unused (always 0?)
3-0  Clockwise rotation in 30 degree steps (00h..0Bh)
```

Rotation around Z-axis is determined by sensing X and Y coordinates of the two
ultrasonic speakers in relation to each other. Examples for the four major
rotation angles are:

```
Val   Clock-face  Degrees  Back-side-of-Hand   Thumb
00h   12:00       0'       Points up           Points left
03h   3:00        +90'     Points right        Points up
06h   6:00        +180'    Points down         Points right
09h   9:00        -90'     Points left         Points down
```

Mind that there are some body-mechanic restrictions:

```
In general, forearm can be be turned clockwise from around 0:00 to 6:00
When using your shoulder you may also go anti-clockwise from 11:00 to 8:00
When breaking your arm, or maybe doing a salto, you might also get to 7:00
```

Note: Rotation around X-axis or Y-axis won't work: The speakers are no longer
aimed at the microphones, so the signals get lost, and the glove cannot compute
position & rotation values.

**5th byte: Finger Flex Sensors**

```
7-6  Thumb (hard to use)   (0..3; 0=Straightest, 3=Most Bent)
5-4  Index Finger          (0..3; 0=Straightest, 3=Most Bent)
3-2  Middle Finger         (0..3; 0=Straightest, 3=Most Bent)
1-0  Ring Finger           (0..3; 0=Straightest, 3=Most Bent)
N/A  Little Finger         (there is no flex sensor for this finger)
```

The glove is automatically calibrating itself to the minimum/maximum flex
positions that it has seen; ie. before using it, one should stretch/bend all
fingers a few times. When not doing that, it defaults to return all 3's (ie.
highest flex seen so far).

Note: The flex sensors seem to be somehow fragile; it appears to be more or
less common that only 3 of the 4 sensors are working (unknown what is causing
that problem; maybe they are broken, or maybe just dirty).

**6th byte: Control Pad Buttons**

```
7-0  Button number of currently pressed button
```

Button numbers are:

```
00h      Keypad "0" or Center (glove beeps & centers when pressing Center?)
01h-09h  Keypad "1".."9"
0Ah      Button A
0Bh      Button B
0Ch      DPAD Left   ;XXX or Up ?
0Dh      DPAD Up     ;XXX or Right ?
0Eh      DPAD Down
0Fh      DPAD Right  ;XXX or Left ?
80h      Enter (glove beeps when pressing this button?)
82h      Start
83h      Select
84h (?)  PROG (glove goes into program-mode when pressing this button)
FFh (?)  None (no button pressed)
```

There can be only one button pressed. Priority order is reportedly (starting
with highest priority):

```
Left,Up,Down,Right, Enter,Start,Select,Center, 0,1,2,3,4,5,6,7,8,9, A,B.
```

Center and Enter are to-be-avoided as they beep the glove and wreck (?) one
sample or so of data. WHEREAS, "wrecking" sounds unlikely... maybe it does
rather stay not-ready for a while during centering? "0" and "Center" are
returning the same value according to two sources (but both being unaware of
the "ready" status-byte; so maybe they are seeing status=00h=centering/not
ready, rather than seeing button=00h=center).

**7th byte: Unknown/unused**

**8th byte: Unknown/unused**

These two bytes seem to be always 00h. Purpose unknown.

**9th byte: Error Flags**

```
7-6  Unused (always 0)
5    Right speaker (on little finger) to Sensor 3 (lower-right mic) (1=Okay)
4    Right speaker (on little finger) to Sensor 2 (upper-right mic) (1=Okay)
3    Right speaker (on little finger) to Sensor 1 (upper-left mic)  (1=Okay)
2    Left speaker (on index finger)   to Sensor 3 (lower-right mic) (1=Okay)
1    Left speaker (on index finger)   to Sensor 2 (upper-right mic) (1=Okay)
0    Left speaker (on index finger)   to Sensor 1 (upper-left mic)  (1=Okay)
```

Indicates if the speaker pings have reached the microphones. Should be 3Fh when
everything is fine, otherwise coordinate & rotation bytes may contain
garbage and one should ignore them.

Note: The six LEDs in the receiver unit seem to be wired to the serial joypad
data. So, after reading the last byte of a packet, the LEDs should reflect
Bit0-3 and Bit6-7 of the Error Flag byte (until the NMI handler reads the
following "Not Ready" byte in next frame).

## Controllers - Power Glove Drawings

```
.-------. sensor 1                           sensor 2 .-------..__
|  .-.  |_____________________________________________|  .-.  |   ''. LEDs:
| ((O)) |_____________________________________________| ((O)) |  o  |  up
|  '-'  | upper-left                      upper-right |  '-'  | o o | lt rt
'-------' microphone                       microphone '-------'  o  |  dn
.-----------------------------------------------------.  |     |
| .----------------------------------.      ___       |  | o o | rapid
| |                                  |    .'   '.     |  |_____| fire
| |                                  |   |   //  |    |    | |   LEDs
| |                                  |   |  //   |    |    | |
| |             TV-Set               |    '.___.'     |    | |
| |                                  |                |    | |
| |                                  |   ( )    ( )   |    | |
| |                                  |                |    | |
| |                                  |  ::::::::::::: |    | |
| |                                  |  ::::::::::::: |    | |
| |                                  |  ::::::::::::: |    | |
| |                                  |  ::::::::::::: |    | | sensor 3
| |                                  |                | .--'-'--.
| '----------------------------------'                | |  .-.  |
|_____________________________________________________| | ((O)) |###,
lower-right |  '-'  |   #
,###########################,        microphone '-------'   #
#"          long cable       "##,                            #
.------"------.                        "############################"
|  junction   |
.##|     box     :

## Controllers - UForce

[Controllers - UForce I/O](#controllersuforceio)

[Controllers - UForce Drawings](#controllersuforcedrawings)

[Controllers - UForce Games and Game Switches](#controllersuforcegamesandgameswitches)

**How the UForce Works (directly from the manual)**

"UForce uses an array of sensors to create a three-dimensional Power Field
about 8 to 10 inches above or in front of each sensor. The Power Field senses
the position and motion of objects within it by combining information from all
its sensors."

Note: The so-called "Power Field" consists of 9 infra-red transmitters, each
one bundled with an infra-red receiver, meant to sense reflections from the
player's hands.

## Controllers - UForce Drawings

**UForce Main Unit (with sensor numbers as ordered in the 8-byte data packet)**

```
.---- sensor #8
/
_______________________ _______ _______________________
|  .                    | ##### |                    .  |
|  |    | | ..mmM       |       |                    |  |
|  |    |_| FORCE       |       |                    |  |
|  |                   /         \                   |  |
|  |                  /           \                  |  |
|  |________________ /             \ ________________|  |
|   _________________________________________________   |
|  |                                                 |  |
sensor |  |'''.                                         .'''|  | sensor
#6---> |  |    :---------------------------------------:    |  |  |  |    :---------------------------------------:    |  |  |  |    :---------------------------------------:    |  |  |  |    :--------------- | ( ) | ---------------:    |  |    \                For use with...              /    |_|      _______________________________      |_|     |   |                  |   |   \              |   |     |  /  |                 |   |                 |  \  |

## Controllers - Barcode Readers

**Datach - Joint ROM System (Bandai) (1992)**

A mini-cartridge adaptor with built-in barcode reader. The device connects to
cartridge slot, and can be used (only) with seven special mini-cartridge games
(six of them actually supporting the barcode feature):

```
Dragon Ball Z: Gekito Tenkaichi Budokai (December 1992)
Ultraman Club: Spokon Fight!! (April 1993)
SD Gundam: Gundam Wars (April 1993)
Crayon Shin-Chan: Orato Poi Poi (August 1993) (this WITHOUT barcode support)
Yu Yu Hakusho: Bakuto Ankoku Bujutsue (October 1993)
Battle Rush: Build Up Robot Tournament (November 1993)
J League: Super Top Players (April 1994)
```

The barcode reader hardware can see only one pixel at a time:

```
[6000h].R.Bit3 Barcode Sensor (0=Black, 1=White)
```

The data seen during scanning is:

```
All Black         ;no card inserted
61 pixels White   ;leading white space (ca. 61 pixels on included cards)
95 pixels Stripes ;barcode (95 pixels for EAN-13 and UPC-A codes)
61 pixels White   ;ending white space (ca. 61 pixels on included cards)
All Black         ;no card inserted
```

Example: In Dragon Ball Z, barcode 062982-144233 gives HP:51500, BP:32500,
DP:25000.

The included cards measure roughly 8.55cm x 5.9cm each. The existing games
support UPC-A and EAN-13 barcodes (both have 30 black stripes), EAN-8 barcodes
(22 black stripes), and, an unknown variable-length barcode format (with 27 or
more black stripes). UPC-E barcodes (17 black stripes) aren't supported.

The scanning software uses 8bit counters (incremented at 73 clk rate), for a
reasonable resolution & avoiding overflows (on wide 4pixel stripes), the
scanning time should be around 300..4600 clks per pixel.

The games are using a variant of "Mapper 16", but apparently with VRAM instead
of VROM. The VRAM has unknown size, and it's probably contained in the "Datach"
adaptor.

Mapper 16: Bandai - PRG/16K, VROM/1K, IRQ, EPROM

Note: Bandai also made a similar mini-cartridge device (named Sufami Turbo) for
the Super Famicom; that device features two cartridge slots, but doesn't
include a barcode reader.

**Barcode Battler II (Epoch) (1992)**

A handheld console with built-in barcode reader and very simple LCD display.
The device can be used as standalone handheld console, or as external barcoder
reader for other consoles. Supported by only one Famicom game:

```
Barcode World (1992) Sunsoft (JP) (includes cable with 15pin connector)
```

Note: The Barcode Battler II is also supported by a couple of SNES games.

Controllers - Barcode Battler (barcode reader)

Controllers - Barcode Battler Transmission I/O

Controllers - Barcode Battler Drawings

In short: The Barcode Battler outputs barcodes as 20-byte ASCII string, at 1200
Baud, 8N1. The NES software receives that bitstream via Port 4017h.Bit2.

**Barcode Format**

Controllers - Barcode Formats

## Controllers - Barcode Battler Transmission I/O

The Barcode Battler outputs barcodes as 20-byte ASCII string, at 1200 Baud,
8N1. The NES software receives that bitstream via Port 4017h.Bit2. The SNES
software requires a BBII Interface, which converts the 8bit ASCII digits into
4bit nibbles, and inserts SNES controller ID and status codes, the interface
should be usually connected to Controller Port 2 (although the existing SNES
games seem to accept it also in Port 1).

**Barcode Battler (with BBII Interface) SNES Controller Bits**

```
1st..12th   Unknown/unused (probably always 0=High?)
13th..16th  ID Bits3..0          (MSB first, 1=Low=One) (must be 0Eh)
17th..24th  Extended ID Bits7..0 (MSB first, 1=Low=One) (must be 00h..03h)
(the SNES programs accept extended IDs 00h..03h, unknown
if/when/why the BBII hardware does that send FOUR values)
25th        Status: Barcode present (1=Low=Yes)
26th        Status: Error Flag 1 ?
27th        Status: Error Flag 2 ?
28th        Status: Unknown      ?
```

Following bits need/should be read ONLY if the "Barcode Present" bit is set.

```
29th-32th   1st Barcode Digit, Bits3..0  (MSB first, 1=Low=One)
33th-36th   2nd Barcode Digit, Bits3..0  (MSB first, 1=Low=One)
37th-40th   3rd Barcode Digit, Bits3..0  (MSB first, 1=Low=One)
41th-44th   4th Barcode Digit, Bits3..0  (MSB first, 1=Low=One)
45th-48th   5th Barcode Digit, Bits3..0  (MSB first, 1=Low=One)
49th-52th   6th Barcode Digit, Bits3..0  (MSB first, 1=Low=One)
53th-56th   7th Barcode Digit, Bits3..0  (MSB first, 1=Low=One)
57th-60th   8th Barcode Digit, Bits3..0  (MSB first, 1=Low=One)
61th-64th   9th Barcode Digit, Bits3..0  (MSB first, 1=Low=One)
65th-68th   10th Barcode Digit, Bits3..0 (MSB first, 1=Low=One)
69th-72th   11th Barcode Digit, Bits3..0 (MSB first, 1=Low=One)
73th-76th   12th Barcode Digit, Bits3..0 (MSB first, 1=Low=One)
77th-80th   13th Barcode Digit, Bits3..0 (MSB first, 1=Low=One)
81th and up Unknown/unused
Above would be 13-digit EAN-13 codes
Unknown how 12-digit UPC-A codes are transferred   ;\whatever leading
Unknown if/how 8-digit EAN-8 codes are transferred ; or ending padding?
Unknown if/how 8-digit UPC-E codes are transferred ;/
```

For some reason, delays should be inserted after each 8 bits (starting with
24th bit, ie. after 24th, 32th, 40th, 48th, 56th, 64th, 72th bit, and maybe
also after 80th bit). Unknown if delays are also needed after 8th and 16th bit
(automatic joypad reading does probably imply suitable delays, but errors might
occur when reading the ID bits via faster manual reading).

**Barcode Battler RAW Data Output**

Data is send as 20-byte ASCII string. Bytes are transferred at 1200 Bauds:

```
1 Start bit (must be 1=LOW)
8 Data bits (LSB first, 1=LOW=Zero, 0=HIGH=One)
1 Stop bit  (must be 0=HIGH)
```

The first 13 bytes can contain following strings:

```
"nnnnnnnnnnnnn"    ;13-digit EAN-13 code (ASCII chars 30h..39h)
;12-digit UPC-A code (with ending/leading padding?)
"     nnnnnnnn"    ;8-digit EAN-8 code (with leading SPC-padding, ASCII 20h)
;8-digit UPC-E code (with ending/leading padding?)
"ERROR        "    ;indicates scanning error
```

The last 7 bytes must contain either one of following ID strings:

```
"EPOCH",0Dh,0Ah    ;

## Controllers - Barcode Formats

**Common Barcode Formats**

```
EAN-13 13-digits in 12 symbols (extra digit encoded in parity)
UPC-A  12-digits in 12 symbols
EAN-8  8-digits in 8 symbols
UPC-E  8-digits in 6 symbols (extra digits encoded in parity)
```

**Barcode Symbols**

Each symbol consists of 7 pixels with 4 bars: White-Black-White-Black in first
half, and vice-versa in second half; this allows to determine the scanning
speed per symbol. There are 3 possible symbols (with inverse and/or reverse
pixels) for each digit:

```
Digit     Left/Odd  Left/Even  Right/Even
0         ...##.#   .#..###    ###..#.
1         ..##..#   .##..##    ##..##.       "." = White
2         ..#..##   ..##.##    ##.##..       "#" = Black
3         .####.#   .#....#    #....#.
4         .#...##   ..###.#    #.###..
5         .##...#   .###..#    #..###.
6         .#.####   ....#.#    #.#....       Left Sync Mark:    #.#
7         .###.##   ..#...#    #...#..       Center Sync Mark:  .#.#.
8         .##.###   ...#..#    #..#...       Right Sync Mark:   #.#
9         ...#.##   ..#.###    ###.#..
```

12-digit UPC-A (and 8-digit EAN-8) barcodes use "Left/Odd" for the first 6 (or
4) symbols, and "Right/Even" for the remaining 6 (or 4) symbols. The
"Left/Even" symbols are used only for 13-digit EAN-13 and 8-digit UPC-E
barcodes (see below).

**UPC-A (12-digits in 12 symbols)**

This is the original barcode format, used mainly in North America, but also
found on products that imported/exported to/from that area (namely, on many
Audio CDs).

**EAN-13 (13-digits in 12 symbols; extra digit encoded in parity)**

This is an extension of the UPC-A format, with an additional leading digit (if
the digit is "0" then it's an UPC-A barcode, otherwise an international EAN-13
barcode).

EAN-13 barcodes are having only 12 symbols (as UPC-A), the 13th digit is
encoded in the "parity" of the 2nd..6th symbol; the other symbols are fixed:
1st=Odd and 12th=Even allow to determine scanning direction; 7th..11th=Even
aren't containing any special information.

The parity (E=Left/Even, O=Left/Odd) for the 1st..6th symbol (aka 2nd..7th
digit) depends on the 1st digit:

```
Digit  1st=0  1st=1  1st=2  1st=3  1st=4  1st=5  1st=6  1st=7  1st=8  1st=9
Parity OOOOOO OOEOEE OOEEOE OOEEEO OEOOEE OEEOOE OEEEOO OEOEOE OEOEEO OEEOEO
```

The parity for the 7th..12th symbol (aka 8nd..13th digit) is fixed (always
Right/Even).

**EAN-8 (8-digits in 8 symbols)**

A short barcode for international use. Encoded as UPC-A, but only 4 symbols in
each half.

**UPC-E (8-digits in 6 symbols; extra digits encoded in parity)**

This is a compressed UPC-A barcode; with the "middle zeros" removed from the
manufacturer/product number field. Unlike UPC-A, these barcodes are rarely
found outside of North America.

```
UPC-E        UPC-A
tMmpppXc --> tMmX0000pppc  ;with X=0..2   ;\for UPC-E, first digit (t)
tMmmpp3c --> tMmm00000ppc                 ; may be 0..1 only),
tMmmmp4c --> tMmmm00000pc                 ; last digit (c) must be checksum
tMmmmmXc --> tMmmmm0000Xc  ;with X=5..9   ;/of the "decompressed" UPC-A code
```

The first digit (0 or 1), and the last digit (checksum, 0..9) are encoded as
"parity" of the six symbols:

```
Digit  8th=0  8th=1  8th=2  8th=3  8th=4  8th=5  8th=6  8th=7  8th=8  8th=9
1st=0  EEEOOO EEOEOO EEOOEO EEOOOE EOEEOO EOOEEO EOOOEE EOEOEO EOEOOE EOOEOE
1st=1  OOOEEE OOEOEE OOEEOE OOEEEO OEOOEE OEEOOE OEEEOO OEOEOE OEOEEO OEEOEO
Left Sync Mark: #.#    Center Sync Mark: None    Right Sync Mark: .#.#.#
```

Whereas, E=Left/Even, O=Left/Odd (Right/Even symbols aren't used by UPC-E).

**Checksums**

The barcode checksum is located in the last digit. This value must be chosen so
that the sum of all digits, plus twice the sum of each second digit sums up to
a decimal value ending with zero.

```
EAN-13 Checksum Weighting: 1-313131-313131     ;1=counted once
UPC-A  Checksum Weighting:   313131-313131     ;3=counted thrice
EAN-8  Checksum Weighting:     3131-3131
UPC-E  Decompress UPC-E to UPC-A, then do UPC-A checksumming
```

**Notes**

Scanning direction can be determined by checking "Odd/Even" parity of the first
& last symbol.

Error checking can be done by verifying the checksum digit and by rejecting any
invalid symbols.

## Controllers - Microphones

**Standard Famicom Microphone**

On older Famicoms, the second control pad (that without Start and Select
buttons) has a microphone with volume control built-in. The signal goes to Bit
2 of Port 4016h (simple 1bit input, not an analogue ADC-converted input).

The signal is also merged with the PPUs sound output signals, as such, allowing
to use the television set/speaker as amplifier/megaphone.

Games that do use the Standard Microphone

```
* Atlantis no Nazo (NES = Super Pitfall II) - get the microphone power-up
and then yell into the microphone to freeze and kill most enemies.
* Hikari Shinwa: Palutena no Kagami (NES = Kid Icarus) - talk into the
microphone to bargain for lower shop prices.
* Raid on Bungeling Bay (NES = Raid on Bungeling Bay) - ?
* SD Kamen Rider -- in one of the mini games, blow air into the microphone
to get a windmill to spin.
* Takeshi no Chousenjou - microphone section, where players are required to
sing a verse of karaoke or talk whilst playing a pachislo minigame (the
microphone section was replaced in later versions of the game once the
microphone was dropped from the Famicom).
* Zelda no Densetsu: The Hyrule Fantasy (cart/disk) (NES = The Legend of
Zelda) - yell into the microphone to kill Pols Voice enemies.
```

Note: Many Coconuts games do also contain a Famicom Microphone reading
function: I Love Softball, Pachio Kun 1-6, Pachinko Daisakusen 1-2 (though
unknown when/if/which games do actually use that function).

**Bandai Microphone**

Cartridge with attached "stage" microphone, two 2 push buttons (A and B), and
ROM expansion slot.

[Mapper 188: UNROM-reversed](./cartridgesandmappers.md#mapper188unromreversed)

**NES LaserScope / Famicom Gun Sight (Konami)**

This is basically a regular "Zapper" lightgun in form of a headset. The thing
contains a microphone that replaces the Zapper's trigger button; thus allowing
"voice activated" shooting.

## Controllers - Arcade Machines

**VS Unisystem**

Arcade Machine with additional coin-detection and DIP-switch inputs. Joysticks
and Lightguns are accessed slightly different as on NES.

[VS System](./vssystem.md)

**Play Choice 10**

Arcade Machine with additional coin-detection, DIP-switch, and game-select
inputs. The inputs are controlled by a separate Z80 CPU. More info:

[Nintendo Playchoice 10](./nintendoplaychoice10.md)

**FamicomBox**

For hotel rooms.

[FamicomBox](./famicombox.md)

## Storage Turbo File

The Turbo File is an external battery-backed RAM-Disk made by ASCII. The device
connects to 15pin Famecom expansion port, accordingly, it's been sold only in
japan, and it's mainly supported by ASCII's own games.

**Turbofile (AS-TF02)**

Original version, contains 8Kbytes battery backed RAM, and a 2-position PROTECT
switch, plus a LED (unknown purpose).

**Turbo File II (TFII)**

Newer version, same as above, but contains 32Kbytes RAM, divided into four
8Kbyte slots, which can be selected with a 4-position SELECT switch.

**Turbo File Adapter**

Allows to connect a Turbo File or Turbo File II to SNES consoles. Aside from
the pin conversion (15pin NES to 7pin SNES), it does additionally contain some
electronics (for generating a SNES controller ID, and a more complicated
protocol for entering the data-transfer phase). Aside from storing SNES game
positions, this can be also used to import NES files to SNES games.

**Turbo File NES I/O Ports**

```
4016h.Write.Bit0 = Data.Out (must be same as OLD data when READING data)
4016h.Write.Bit1 = Reset Address to Offset 0000h
4016h.Write.Bit2 = Data.Clock
4017h.Read.Bit2  = Data.In (can be ignored when WRITING data)
```

To reset the address to offset 0000h:

```
[4016h]=00h           ;reset address to zero
[4016h]=02h           ;release reset
```

To read a bit (and to write-back the same unchanged value):

```
old=[4017h].bit2      ;get old data
[4016h]=old+06h       ;output data and clk=high
[4016h]=old+02h       ;output data and clk=low
```

To write a bit:

```
[4016h]=new+06h       ;output data and clk=high
[4016h]=new+02h       ;output data and clk=low
```

Bytes are usually transferred LSB first. Except, the oldest game (Castle
Excellent from 1986) did use MSB first. The address increments after each 8
bits. To seek a specific address: Reset address to 0000h, then issue dummy
reads until the desired address is reached.

**Turbo File Memory**

The first byte (at offset 0000h) is unused (possibly because that there is a
risk that other games with other controller access functions may destroy it);
after resetting the address, one should read one dummy byte to skip the unused
byte. The used portion is 8191 bytes (offset 0001h..1FFFh). The "filesystem" is
very simple: Each file is attached after the previous file, an invalid file ID
indicates begin of free memory (though SOME games seem to keep searching for
valid IDs even after that point?).

**Turbo File Fileformat (newer files) (1987 and up)**

Normal files are formatted like so:

```
2   ID "AB" (41h,42h)
2   Filesize (16+N+2) (including title and checksum)
16  Title in ASCII (terminated by 00h or 01h)
N   Data Portion
2   Checksum (all N bytes in Data Portion added together)
```

**Turbo File Fileformat (old version) (1986)**

The oldest Turbo File game (Castle Excellent from 1986) doesn't use the above
format. Instead, it uses the following format, without filename, and with
hardcoded memory offset 0001h..01FFh (511 bytes):

```
1   Don't care (should be 00h)    ;fixed, at offset 0001h
2   ID AAh,55h                    ;fixed, at offset 0002h..0003h
508 Data Portion (Data, end code "BEDEUTUN", followed by some unused bytes)
```

CAUTION:

```
The early version has transferred all bytes in reversed bit-order,
so above ID bytes AAh,55h will be seen as 55h,AAh in newer versions!
```

Since the address is hardcoded, Castle Excellent will forcefully destroy any
other/newer files that are located at the same address. Most newer NES/SNES
games (like NES Fleet Commander from 1988, and SNES Wizardry 5 from 1993) do
include support for handling the Castle Excellent file. One exception that
doesn't support the file is NES Derby Stallion - Zenkoku Ban from 1992.

**Deleting Files**

Delting files would require to relocate all following files - since the NES has
only 2K WRAM, this would require to do the relocation in smaller blocks - which
is simply not supported by most games. So far, the only way for deleting files
is to remove the batteries (wheras it may take some minutes until the data is
lost).

NES Derby Stallion - Zenkoku Ban (1992) does have some limited delete-support:
If there isn't free enough memory, then it does allow to erase the last-most
file(s); which can be done without relocations.

SNES Wizardry 5 (1993) does allow to delete individual files (the SNES has 128K
WRAM, so relocating the following files is pretty simple).

**NES Games that do support the Turbo File / Turbo File II**

```
Best Play Pro Yakyuu (1988) ASCII (J)
Best Play Pro Yakyuu '90 (1990) (J)
Best Play Pro Yakyuu II (1990) (J)
Best Play Pro Yakyuu Special (1992) (J)
Castle Excellent (1986) ASCII (J) (early access method without filename)
Derby Stallion - Zenkoku Ban (1992) Sonobe Hiroyuki/ASCII (J)
Downtown - Nekketsu Monogatari (19xx) Technos Japan Corp (J)
Dungeon Kid (1990) Quest/Pixel (J)
Fleet Commander (1988) ASCII (J)
Haja no Fuuin (19xx) ASCII/KGD (J)
Itadaki Street - Watashi no Mise ni Yottette (1990) ASCII (J)
Ninjara Hoi! (J)
Wizardry - Legacy of Llylgamyn (19xx?) (J)
Wizardry - Proving Grounds of the Mad Overlord (1987) (J)
Wizardry - The Knight of Diamonds (1991) (J)
```

NES games that do support Turbo File should have a "TF" logo on the cartridge.

Plus... (?)

Searching for: ad17404a4a290109068d1640 (late_nes_recv_joy2_byte opcodes)

Searching for: ad174029044a4a09068d1640 (newer_nes_recv_joy2_byte opcodes)

```
!! Best Keiba - Derby Stallion (1991) Sonobe Hiroyuki/ASCII (J)
!! Famicom Shougi - Ryuuousen (1991) I'MAX/HOME DATA (J)
!! Money Game 2 - Kabutochou no Kiseki, The (1989) Sofel Ltd (J)
!  Castlequest (U)           US VERSION of Castle Excellent (FUNCTIONAL???)
!  Kunio 8-in-1 [p1]         (pirate multicart, probably contains a TF-game?)
```

**SNES Games that support Turbo File Adapter & Turbo File Twin in TFII-Mode**

```
Ardy Lightfoot (1993)
Derby Stallion II (1994)
Derby Stallion III (1995) (supports both TFII and STF modes)
Derby Stallion 96 (1996) (supports TFII and STF and Satellaview-FLASH-cards)
Derby Stallion 98 (NP) (1998) (supports both TFII and STF modes)
Down the World: Mervil's Ambition (1994)
Kakinoki Shogi (1995) ASCII Corporation
Tactics Ogre - Let Us Cling Together (supports both TFII and STF modes)
Wizardry 5 - Heart of the Maelstrom (1992) Game Studio/ASCII (JP)
BS Wizardry 5 (JP) (Satellaview BS-X version)
```

Note: The US version of Wizardry 5 (1993) contains 99% of the turbo file
functions, but lacks one opcode that makes the hardware detection
nonfunctional. Wizardry 5 was announced to be able to import game positions
from NES to SNES.

**Turbo File Schematic**

```
.--------. 74HC368     .---||--GND    .--------.        D0-----PORT1.2
GND---|1A   /Y1|---NC        | 680pF        |4024  Q1|---NC
OUT2---|2A   /Y2|-------------o-----/OUT2    |      Q2|---NC
/OUT2---|3A   /Y3|--------[100]--o------------|/CLK  Q3|---NC
GND---|/OE1    |               |1000pF      |      Q4|-------.
| - - - -|               '--||-GND    |      Q5|---NC  |
OUT1---|4A   /Y4|-----------------------o----|RST   Q6|---NC  |
OUT0---|5A   /Y5|----------.            |    |      Q7|---NC  |
GND---|6A   /Y6|---NC     |            |    '--------'       |
/OUT2---|/OE2    |          |            | .-------------o-----'    SRAM 8Kx8
'--------'          |            | |             | .-------.LH5164L-10
OUT1-----[10K]---GND       |            | |  .--------. '-|A0   NC|--NC
OUT2-----[10K]---GND       |            | |  |4040  Q1|---|A1   D0|--D0
.--------. 74HC573  |            | |  |      Q2|---|A12  D1|--D1
/OUT2---|G       |          |            | '--|/CLK  Q3|---|A7   D2|--D2
/OUT2---|/OE     |          |            |    |      Q4|---|A6   D3|--D3
D7---|D0    Q0|---D6     |            '----|RST   Q5|---|A4   D4|--D4
D6---|D1    Q1|---D5     |                 |      Q6|---|A3   D5|--D5
D5---|D2    Q2|---D4     |                 |      Q7|---|A5   D6|--D6
D4---|D3    Q3|---D3     |                 |      Q8|---|A11  D7|--D7
D3---|D4    Q4|---D2     o OFF             |      Q9|---|A10 /OE|--GND
D2---|D5    Q5|---D1   \    PROTECT        |     Q10|---|A9  /WE|--/OUT2
D1---|D6    Q6|---D0    \   SWITCH         |     Q11|---|A8  /CE|--/GOOD
D0---|D7    Q7|-------o  o-----------D7    |     Q12|---|A2  CE2|--OUT1
'--------'       ON                   '--------'   '-------'
Plus, a bunch of transistors, resistors, diodes that control supply and
battery, power LED, and power /GOOD signal. Battery standby supply is
wired to the SRAM, and also to the 74HC368.
Note: Turbo File II uses 32Kx8 SRAM which lacks CE2 pin, so schematic
may be a bit different; and of course, A13 and A14 are somehow wired
to the 4-position switch for 8K bank selection.
```

## Storage Battle Box I/O Access

**Battle Box**

```
[4016h].W.Bit0 --> CLK to EEPROM (and, when set, enable chipselect toggle)
[4017h].R      --> toggle DATA to EEPROM (always)
[4017h].R      --> toggle 1st/2nd EEPROM chipselect (when [4016h].W.Bit0=1)
[4017h].R.Bit3  Read
01100000  06h      60h --> Program
00110000  0Ch      30h --> Chip Erase
10110000  0Dh      B0h --> Busy Monitor
10010000  09h      90h --> Erase/Write Enable
11010000  0Bh      D0h --> Erase/Write Disable
```

**battle_box_command(cmd,addr,data)**

```
if cmd<>B0h then battle_box_command(B0h,addr,0)         ;-
[4016h]=01h              ;OUT0=1                        ;\start transfer,
dummy=[4017]             ;toggle chipsel                ; toggle chipsel
if (addr AND 1)<>battle_box_chipsel then dummy=[4017]   ; depending on addr.0
battle_box_chipsel = (addr AND 1)                       ;/
battle_box_send_byte(((addr/2) AND FEh)+(addr AND 01h)) ;\send addr/cmd
battle_box_send_byte(cmd+addr/200h)                     ;/
if cmd=B0h  ;Busy Mode                                  ;\wait busy (if any)
wait until battle_box_recv_bit=0                      ;/
if cmd=80h  ;Read Word                                  ;\
data1st=battle_box_recv_byte                          ; read data (if any)
data2nd=battle_box_recv_byte                          ;/
if cmd=60h  ;Write Word                                 ;\
battle_box_send_byte(data1st)                         ; write data (if any)
battle_box_send_byte(data2nd)                         ;/
dummy=[4017h]            ;toggle chipsel                ;\finish transfer
[4016h]=00h              ;OUT0=0                        ;/
```

**battle_box_detect_which_chip_is_selected:**

```
battle_box_command(80h,000h,data)  ;read ID ("B"=42h, or "X"=58h) from addr 0
battle_box_chipsel=battle_box_chipsel XOR data.bit3
```

**battle_box_send_byte(data)**

```
data=data XOR FFh                ;-invert
for i=7 downto 0
[4016h]=00h              ;OUT0=0
dummy=[4017h]            ;toggle DTA.OUT and get output level
if dummy.bit4<>data.bit(i) then dummy=[4017h] ;toggle DTA.OUT
[4016h]=01h              ;OUT0=1
next i
```

**battle_box_recv_byte:**

```
for i=7 downto 0
data.bit(i)=battle_box_recv_bit
next i
data=data XOR FFh                ;-invert
```

**battle_box_recv_bit:**

```
[4016h]=00h              ;OUT0=0
dummy=[4017h]            ;read DATA
bit=dummy.bit3
[4016h]=01h              ;OUT0=1
```

**Notes**

The Program command seems to be able to rewrite data without needing any prior
Erase.

After power-on, wait 1ms before accessing the chip. Then send Erase/Write
Enable or Disable as first command.

## Hori Game Repeater

The Game Repeater (GR-7) from Hori is a device that can record and playback
controller data. The data is stored in 16Kbytes of SRAM (assuming that many
games read 1 byte per frame, this allows to record about 5 minutes). The SRAM
isn't battery backed, but, for permanent storage, the SRAM content can be
loaded/saved to external tape recorder.

The device can be used to create "movies" that play back a whole level, and it
may be also useful for recording short button-sequences for getting through
difficult sections of a game. The playback function may obviously fail if a
game contains random elements (such like starting with different random seeds).

The recorded data is taken from the controller input, so it should be
impossible to <output> data from the console (for using the SRAM as
expansion memory or writing game positions to tape) (unless the device should
happen to have support for doing that via another controller pin?).

**Chipset/Components**

```
1x Toshiba N (42pin 4bit single-chip CPU: ROM:2048x8, RAM:128x4)
2x Toshiba TC5563APL-15 (28pin static ram: RAM:8192x8)
1x TC4011BP 14pin (quad NAND)
1x TC4062UBP ? (or is it "8"UBP?) 14pin
1x TC4078BP 16pin (is that "8"BP?)
1x TC4021BP 16pin (8bit parallel-in, serial-out shift register)
1x NEC D4094BC ? 16pin (8bit serial-in, parallel-out shift register)
3x small buttons (tape recorder LOAD,SAVE,VERIFY)
1x bigger button (sampling START)
2x two-position switches (REPEAT/RECORD, and STOP/STANDBY)
2x small LEDs (tape recorder LOAD,SAVE)
2x cassette connector (tape recorder LOAD,SAVE)
1x male dsub 15pin connector (to famicom console)
1x female dsub 15pin connector (to external joystick)
1x DC input socket (from power supply)
1x DC output cable (to famicom console)
1x 7805 ? voltage converter
1x unknown thing/unknown purpose (on front side) (connector? dip-switches?)
```

Plus one XTAL, plus resistors/diodes/capacitors. There's no battery on the SRAM
chips.

## R.O.B. (Robotic Operating Buddy)

R.O.B. (Robotic Operating Buddy) is a small wireless plastic robot, about 24 cm
in height. Command transmission from console to robot is done via flashing TV
frames. There is no direct feedback from the robot to the console (except that,
in Gyromite, the robot can be maneuvered to push buttons on joypad 2).

**R.O.B. Games**

The Robot is used by two games:

```
Gyromite (Robot Gyro) (1985) (JP) (US) (EU)
Stack Up (Robot Block) (1985) (JP) (US)
```

Gyromite is a platform game in which the robot must be used to unlock red/blue
barriers (by dropping heavy spintops on the red/blue trays). In Stack-Up, one
must stack colored plastic blocks in a specific order.

**Hardware Accessories**

General Accessories:

```
Sun-glasses (dark transparent cover, for compensating brightness of the TV)
4 AA batteries (for powering the robot)
```

Gyromite Accessories:

```
2 robot-gloves with zig-zagged surface (for holding the spinning gyros)
2 gyros (heavy spintops that can be used to depress red/blue trays)
2 red/blue trays (with levers to push buttons on joypad 2)
2 black trays (for depositing gyros when not using them)
1 spinner (accellerates the gyros; for stabilizing them on red/blue trays)
1 battery (for powering the spinner motor)
```

Stack-Up Accessories:

```
2 robot-gloves with flat rubber-coated surface (for holding blocks)
5 differently colored stack-able blocks (to be arranged in specific orders)
5 gray trays (for depositing the blocks on them)
```

**Drawings**

[R.O.B. Technical Drawings](#robtechnicaldrawings)

**Robot Motors**

```
Left/Right Motor  five stopping points (60'/step, 240' total range)
Up/Down Motor     six stopping points (1.4 cm/step, 7 cm total range)
Open/Close Motor  two stopping points (7 cm/step, 7 cm total range)
```

**CRT-Signals**

```
Command
Left     xxx.xxx.___.___.___.GGG.___.GGG.___.GGG.GGG.GGG.___.GGG.___.xxx.xxx
Right    xxx.xxx.___.___.___.GGG.___.GGG.GGG.GGG.___.GGG.___.GGG.___.xxx.xxx
Up       xxx.xxx.___.___.___.GGG.___.GGG.GGG.GGG.GGG.GGG.___.GGG.___.xxx.xxx
Down     xxx.xxx.___.___.___.GGG.___.GGG.___.GGG.___.GGG.GGG.GGG.___.xxx.xxx
Open     xxx.xxx.___.___.___.GGG.___.GGG.GGG.GGG.___.GGG.GGG.GGG.___.xxx.xxx
Close    xxx.xxx.___.___.___.GGG.___.GGG.___.GGG.GGG.GGG.GGG.GGG.___.xxx.xxx
Test     ___.GGG.___.GGG.___.GGG.___.GGG.___.GGG.___.GGG.___.GGG.___.GGG.___
Picture  xxx.xxx.xxx.xxx.xxx.xxx.xxx.xxx.xxx.xxx.xxx.xxx.xxx.xxx.xxx.xxx.xxx
```

Whereas, "xxx"=Picture Frame, "___"=Black Frame, "GGG"=Green Frame "."=Vblank.

**Joypad 1 (user input)**

In Gyromite, the joypad buttons are assigned as follows (in one game mode, the
joypad is also used to move the player character on the screen, in that mode,
Start-button must be pressed prior to each robot-command):

```
Left/Right  --> Turn Shoulders & Arms & Shaft Left/Right (in 60' steps)
Up/Down     --> Move Shoulders & Arms Up/Down (in 1.4cm steps)
Button A    --> Open Arms (Release)
Button B    --> Close Arms (Grab)
```

Stack-Up uses some sort of GUI to enter commands, so there's no direct relation
between joypad-buttons and motors in that game.

**Joypad 2 (attached to Gyromite Trays, for feedback from robot to console)**

In Gyromite, dropping spintops on trays results in following feedback:

```
Red-Tray (Slot 2)  ---> lever pushes B-Button on Joypad 2
Blue-Tray (Slot 3) ---> lever pushes A-Button on Joypad 2
```

There's no automatic feedback in Stack-Up (the user must manually push
Start-Button on Joypad 1 once when having finished a level).
