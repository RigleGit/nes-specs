# Nonescontrols

> Source: https://problemkaputt.de/everynes.htm
> Section: Nonescontrols

No$nes Controls 
**General Joypad Controls**

Keyboard controls for up to four players can be defined in setup. Buttons of
USB Gamepads & Joysticks & the like can be also changed per player.

**Special Controls**

```
F1..F5  Enable/Disable Sound Channel 1-5
F9      Enable/Disable Showing Screen Splits as dotted line
NUM *   Hard Reset
NUM /   Soft Reset
NUM -   Switch to Debug Mode
NUM +   Disable Realtime Delays and use 10% frameskip (while held down)
BS      Disable Realtime Delays (same as above, for notebook keyboards)
```

**Famicom Disk System (FDS)**

```
INS     Flip Disk (some games ignore fast flips; hold down for some seconds)
```

**VS Unisystem (Arcade Cabinet)**

```
INS       Credit Left Coin Slot
HOME      Credit Right Coin Slot
PGUP      Credit Service Button
1..4      Button 1..4 (aliases for NES Start/Select buttons)
0         DIP-Switch Window
```

**Playchoice 10 (Arcade Cabinet)**

```
INS       Credit Coin Slot 1
HOME      Credit Coin Slot 2 (works only if enabled via DIP switches)
PGUP      Service Button (add credit) (during reset: bookkeeping)
DEL       Reset Button (if any)
END       Enter Button (also works via Num-Enter)
PGDN      Channel Select Button
0         DIP-Switch Window
Mouse     Zapper (Lightgun Move/Trigger)
```

**FamicomBox**

```
INS         XXX Insert Coin
HOME      RESET Button
PGUP        XXX GAME/TV Button
123456    1st..6th Keyswitch Position (from left)     [=INTENDED, see Note]
0         DIP-Switch Window
Mouse     Zapper (Lightgun Move/Trigger)
```

Note: The ordering of the keyswitch positions from left-to-right is still
unknown; currently keys 1..6 are simply mapped to keyswitch bit0..5.

Multi-cart loading isn't emulated yet, so you'll only see the menu &
self-test feature, without any games.

**Famicom Keyboard**

```
BS/DEL  DEL
HOME    CLR
TAB     ESC
END     STOP
L-ALT   GRPH
R-ALT   KANA
```

**Party Tap Push Buttons**

```
123456  Buttons for Player 1-6
```

**Konami Hyper Shot Push Buttons**

```
Joypad Button A --> "RUN"-Button (or, used as "Shoot-Right" in Hyper Sports)
Joypad Button B --> "JUMP"-Button (or, used as "Shoot-Left" in Hyper Sports)
```

The games do replace joypad data by button data; no$nes somewhat undoes that
replacement.

**Lightguns (NES Zapper, Famicom Beam Gun, VS Zapper, Bandai Hyper Shot Gun)**

```
Mouse                 Move
Left Mouse Button     Trigger
```

**Paddle (NES and both Famicom versions)**

```
Mouse (left/right)    Move
Left Mouse Button     Button
```

**Oeka Kids Tablet**

```
Mouse                 Move & Touch
Left Mouse Button     Click
```

**Mouse / Trackball**

```
Mouse (move)          Move
Left/Right Button     Left/Right Button (A/B Button on Trackball)
Middle Button or ESC  Return mouse to operating system
```

**Power Glove**

```
Gamepad Analog X/Y/Z/R   Position X/Y/Z and Wrist Rotation
Gamepad Button 1..4      Thumb/Index/Middle/Ring Finger Flex
Gamepad Digital POV      Keypad DPAD
Keyboard 0..9            Keypad 0..9
Keyboard Buttons/DPAD    Keypad A,B,Start,Select,DPAD
XXX                      Keypad Prog,Center,Enter
```

**UForce**

```
Page Down         Top-most sensor                         ;\for push-button
NUM-/ NUM-*       Upper Left/Right sensors in upper field ; style usage (with
NUM-8 NUM-9       Lower Left/Right sensors in upper field ; sensor=min/max)
NUM-5             Upper Left       sensor  in lower field ; (eg. Nuclear Rat,
NUM-2 NUM-3       Lower Left/Right sensors in lower field ; and Rock on Air)
Page Up           Bottom-most sensor                      ;/
Gamepad Analog X  Top-most sensor                         ;\for analog usage
Gamepad Analog Y  Bottom-most sensor, or,                 ; (eg. Hose'em Down
Gamepad Analog Y  Lower-Left sensor in lower field        ;/and Rock on Air)
```

Plus Start/Select as configured in joypad setup. Analog mode is activate when
moving the analog gamepad (and will then override the corresponding keyboard
keys). Analog input emulation is matched to the existing "UForce Power Games"
cartridge (and may mismatch with other/homebrew UForce games).

**Top-Rider Bike**

```
Gamepad Analog X         Steering Handles Left/Right
Gamepad Analog Y         Forward=GearLo, Back=GearHi
Gamepad Analog Z         Forward=Accelerate Slow/Fast, Back=Accelerate Turbo
Gamepad Button 1,2,3,4   Brake,Wheelie,Start,Select
```

**Pachinko**

```
Digital Joypad/Keyboard  Pachinko Joypad Buttons
Analog Joypad Forward    Pachinko Analog Dial
```

**Barcode Reader (Barcode Batter and Datach)**

```
INS-Key       Paste numeric ASCII string from clipboard and Send barcode
DEL-Key       Manually Clear input buffer (without sending)
Keypad 0..9   Manually Key-in Barcode digits
Keypad Dot    Manually Confirm input and Send (or re-send) barcode
```

Note: For the Barcode Battler, only EAN-13 is implemented (since it's unknown
how/if it supports shorter UPC-A, UPC-E, or EAN-8 barcodes). For Datach,
EAN-13, UPC-A, EAN-8 are supported (however, the Datach software seems to
support a further non-standard unknown barcode format, which isn't emulated).

**Capcom Mahjong Controller**

```
A..N          Drop Card A..N (or N=Draw new card)
Space         Same as N
F1..F7        Function Keys (Select, Start, and five "japanese" keys)
Tab/LeftCtrl  Same as F1 (Select)
Enter         Same as F2 (Enter)
```

**Piano Keyboard**

```
12345...      1st Octave (12 keys) (only last 7 keys used on Doremikko)
QWERT...      2nd Octave (12 keys) (all keys used)
ASDFG...      3rd Octave (12 keys) (all keys used)
ZXCVB...      4th Octave (10 keys) (only first 5 keys used on Doremikko)
Rshift/Ctrl   4th Octave (last 2)  (Miracle only)
NUM-Dot       5th Octave (1 key)   (Miracle only)
NUM-0..7      Control Buttons 0..7 (Miracle only)
Left Shift    Sustain Foot Pedal   (Miracle only)
```

Note: The miracle sound isn't emulated (as far as known there aren't any dumps
of it's BIOS and SOUND-ROM existing yet). The FDS sound for Doremikko isn't
emulated yet (when I last checked, there seems to have been little known about
FDS sound hardware).

**Mats (Power Pad/Family Trainer/Tap-tap Mat)**

```
QWER --> Upper row (Side B)     RTYU --> Upper Row (Side A, or Tap-tap Mat)
ASDF --> Middle row (Side B)    FGHJ --> Middle Row (Side A, or Tap-tap Mat)
ZXCV --> Lower row (Side B)     VBNM --> Lower Row (Side A, or Tap-tap Mat)
```

Safety Note: Put the keyboard on the floor if you are using feet or hammer.

**Exciting Boxing Bag**

```
Num-7,8,9   Left Hook,   N/A    ,Right Hook
Num-4,5,6   Left Jabb, Straight ,Right Jabb
Num-1,2,3   Left Push,  Body    ,Right Push
```

**RacerMate Bicycle Trainer**

```
Gamepad Analog X   Pulse
Gamepad Analog Y   Speed
Gamepad Analog Z   Watts
F1 or Enter        Button F1 (START)
F2                 Button F2 (DISPLAY)
F3                 Button F3 (SET)
F4 or End          Button RESET
PGUP or Up         Button Plus (UP)
PGDN or Down       Button Minus (DOWN)
```
