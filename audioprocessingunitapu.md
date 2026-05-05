# Audioprocessingunitapu

> Source: https://problemkaputt.de/everynes.htm
> Section: Audioprocessingunitapu

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

Controllers - Microphones

Based on "2A03 technical reference by Brad Taylor" (1st release, April 2004).

APU Channel 1-4 Register 1 (Sweep)

**4001h - APU Sweep Channel 1 (Rectangle)**

**4005h - APU Sweep Channel 2 (Rectangle)**

```
0-2     Sweep right shift amount (S=0..7)
3       Sweep Direction          (0=[+]Increase, 1=[-]Decrease)
4-6     Sweep update rate        (N=0..7), NTSC=120Hz/(N+1), PAL=96Hz/(N+1)
7       Sweep enable             (0=Disable, 1=Enable)
```

At specified Update Rate, the 11bit Wavelength will be modified as such:

```
Wavelength = Wavelength +/- (Wavelength SHR S)
(For Channel 1 Decrease only: minus an additional 1)
(Ie. in Decrease mode: Channel 1 uses NOT, Channel 2 uses NEG)
```

Wavelength register will be updated only if all 3 of these conditions are met:

```
Bit 7 is set (sweeping enabled)
The shift value (which is S in the formula) does not equal to 0
The channel's length counter contains a non-zero value
```

Sweep end: The channel gets silenced, and sweep clock is halted, when:

```
1) current 11bit wavelength value is less than 008h
2) new 11bit wavelength would become greater than 7FFh
```

Note that these conditions pertain regardless of any sweep refresh rate values,
or if sweeping is enabled/disabled (via Bit7).

**4009h - APU N/A Channel 3 (Triangle)**

**400Dh - APU N/A Channel 4 (Noise)**

```
0-7     Unused (No Sweep support for these channels)
```

APU Channel 1-4 Register 3 (Length)

**4003h - APU Length Channel 1 (Rectangle)**

**4007h - APU Length Channel 2 (Rectangle)**

**400Bh - APU Length Channel 3 (Triangle)**

**400Fh - APU Length Channel 4 (Noise)**

Writing to the length registers restarts the length (obviously), and also
restarts the duty cycle (channel 1,2 only), and restarts the decay volume
(channel 1,2,4 only).

```
0-2   Upper 3 bits of wavelength (unused on noise channel)
3-7   Length counter load register (5bit value, see below)
```

The above 5bit value is translated to the actual 7bit counter value as such:

```
Bit3=0 and Bit7=0 (Dividers matched for use with PAL/50Hz)
Bit6-4  (0..7 = 05h,0Ah,14h,28h,50h,1Eh,07h,0Dh)
Bit3=0 and Bit7=1 (Dividers matched for use with NTSC/60Hz)
Bit6-4  (0..7 = 06h,0Ch,18h,30h,60h,24h,08h,10h)
Bit3=1 (General Fixed Dividers)
Bit7-4  (0..F = 7Fh,01h..0Fh)
```

The 7bit counter value is decremented once per frame (PAL=48Hz, or NTSC=60Hz)
the counter and sound output are stopped when reaching a value of zero. The
counter can be paused (and restarted at current location) by Length Counter
Clock Disabled bit in Register 0.

APU Control and Status Registers

**4015h - DMC/IRQ/length counter status/channel enable register**

```
0       Status/Enable rectangle wave channel 1
1       Status/Enable rectangle wave channel 2
2       Status/Enable triangle wave channel 3
3       Status/Enable noise channel 4
4       Status/Enable DMC channel 5
5       Not used (returns garbage on reading)
6       Frame IRQ status (active when set)
7       DMC's IRQ status (active when set)
```

Reading Bit4-0 returns 0 for a zero count status in the length counter
(channel's sound is disabled), and 1 for a non-zero status. Reading Bit7-6
returns IRQ status flags.

Writing to Bit4-0: Writing 0 forces to disable the channel, it will get stopped
and become silent (as if its length counter has reached 0), writing 1
de-activates the forced-stop (without changing the length counter, playback
continues at whatever value have been in the length counter).

Writing to Bit7-5: Unknown.

DMC IRQs are acknowledged by WRITING any value to 4015h.

Frame IRQs are acknowledged by READING from 4015h.

Note that all 5 writable bits in 4015h will be set to 0 upon 2A03 reset.

**4017h - APU Low frequency timer control (W)**

Any write to 4017h resets both the frame counter, and the clock divider.

Sometimes, games will write to this register in order to synchronize the sound
hardware's internal timing, to the sound routine's timing (usually tied into
the NMI code). The frame IRQ frequency is slightly smaller than the PPU's
vertical retrace frequency, so you can see why games would desire this
synchronization.

```
bit6:  Frame IRQ Disable  (0=Enable Frame IRQ, 1=Disable Frame IRQ)
bit7:  Frame Rate Select  (0=NTSC=60Hz=240Hz/4, 1=PAL=48Hz=240Hz/5)
...XXX... Frame IRQ works ONLY if bit6 and bit7 are BOTH zero!
```

On 2A03 reset, Bit6-7 will be cleared. That means that Frame IRQs are enabled
by default (though usually not executed since the CPUs IRQ-Disable-Flag is set
on reset).

Frame IRQs are acknowledged by reading from 4015h.

**Frame Counter**

Several audio timings are referred to as "Frames" and "PAL" and "NTSC",

```
! These timings are NOT physically related to actual PPU VBlank/NMI timings !
```

The Audio "Frame" counter can be switched into "PAL" or "NTSC" mode by software
- regardless of whether the game does run on a PAL/NTSC console. Audio-frames
may be (more or less) synchronized with video-frames by chosing PAL/NTSC
audio-mode matching to PAL/NTSC console type respectively.

The frame counter is based on a 240Hz signal which is gained from dividing the
1.78Mhz PHI2 clock edges (2*1.78M edges/second) by 14915. In PAL Mode (4017h
Bit7=1), the 240Hz signal is divided by 1.25 (by simply leaving out each fifth
clock pulse), resulting in a somewhat dirty 192Hz signal. This PAL/NTSC
adjustment mechanism counts through 4 or 5 steps, producing output as such:

```
0/NTSC: 4,0,1,2,3,0,1,2,3,0,1,2,3  |  1/PAL: 0,1,2,3,4,0,1,2,3,4,0,1,2,3,4
240Hz:  __-_-_-_-_-_-_-_-_-_-_-_-  |  192Hz: -_-_-_-___-_-_-_-___-_-_-_-__
120Hz:  ____-___-___-___-___-___-  |  96Hz:  __-___-_____-___-_____-___-__
60Hz:   (above somehow div by 2)   |  48Hz:  (above somehow divided by 2)
```

Frame Counter is reset on writing to 4017h, and does then restart sequences as
shown above (in NTSC mode starting with a skipped step, whilst directly
starting with a non-skipped step in PAL mode).

Linear counter (triangle) and envelope decay counters (rectangle/noise) are
clocked by 240Hz/192Hz. Frequency sweep (rectangle) clocked by 120Hz/96Hz.
Length counters (all channels) and Frame IRQ clocked by 60Hz/48Hz.

**4014h - SPR-RAM DMA**

Sprite RAM DMA Function contained in 2A03 chip. See PPU description.

**4016h - Write**

Three bit general purpose output latch contained in 2A03 chip.

Used to strobe joysticks. See Controllers chapter for more info.

**4016h/4017h - Read**

The 2A03 chip does not actually contain read-able registers at these addresses,
however, it does output read-request signals for these addresses, which are
used to activate on-board joystick inputs.

See Controllers chapter for more info.

Note: 4015h is the only R/W register in the 4000h-4017h area, all other
registers in this area are write-only, and do not respond to read cycles
(except for the external read-able 4016h/4017h registers).

APU Various

After 2A03 reset, the sound channels are unavailable for playback during the

first 2048 CPU clocks.

The rectangle channel(s) frequency in the range of 54.6 Hz to 12.4 KHz.

The triangle wave channel range of 27.3 Hz to 55.9 KHz.

The random wavelength channel range anywhere from 29.3 Hz to 447 KHz.

**RP2A03E quirk**

I have been informed that revisions of the 2A03 before "F" actually lacked

support for the 93-bit looped noise playback mode. While the Famicom's 2A03

went through 4 revisions (E..H), I think that only one was ever used for the

front loading NES: "G". Other differences between 2A03 revisions are

unknown. Is that Quirk True and Confirmed ?

APU External Sound Channels

The Famicom 60-pin cartridge slot includes a SND_IN pin, allowing external
sound controllers (as included in some Cartridge Mappers, and in Famicom Disk
System) to produce additional sound channels which are merged with the normal
APU channels:

Mapper 5: MMC5 - BANKING, IRQ, SOUND, VIDEO, MULTIPLY, etc.

Mapper 19: Namcot 106 - PRG/8K, VROM/1K/VRAM, IRQ, SOUND

Mapper 20: Disk System - PRG RAM, BIOS, DISK, IRQ, SOUND

Mapper 24: Konami VRC6A - PRG/16K/8K, VROM/1K, NT, IRQ, SOUND

Mapper 26: Konami VRC6B - PRG/16K/8K, VROM/1K, NT, IRQ, SOUND

Mapper 85: Konami VRC7A/B - PRG/16K/8K, VROM/1K, NT, IRQ, SOUND

Mapper 188: UNROM-reversed

However, the NES 72-pin cartridge slot DOES NOT include a SND_IN pin, even
though it does have more (more or less unused) pins than Famicom.

And, there are a few devices with external speakers (not routed through NES
SND_IN) - a "beep" function Power Glove, and fully featured synthesizer in the
Miracle:

Controllers - Piano Keyboards

Controllers - Power Glove

And, the FamicomBox contains a beep function (when money is inserted); the beep
sound is merged with the APU sound, and then passed to TV-Set.

FamicomBox
