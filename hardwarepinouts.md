# Hardwarepinouts

> Source: https://problemkaputt.de/everynes.htm
> Section: Hardwarepinouts

Hardware Pin-Outs
**Pin-Outs**

[Cartridge Pin-Outs](./cartridgesandmappers.md#cartridgepinouts)

[Cartridge Shell Dimensions](./cartridgesandmappers.md#cartridgeshelldimensions)

[Controllers - Pin-Outs](./controllers.md#controllerspinouts)

[Chipset Pin-Outs](#chipsetpinouts)

[NES Expansion Port](#nesexpansionport)

**Upgrading**

[Nocash SRAM Circuit](#nocashsramcircuit)

## NES Expansion Port

**NES Expansion Port, 48-pins, (at bottom of console, rarely used)**

```
Pin            Dir  Expl.
1,48,2,47      Out  VCC,VCC,GND,GND (Supply +5VDC and Ground)
23             Out  VDD voltage from external power supply (usually +10VDC)
3              In   AIN (Audio Input)
21,22,23,24    Out  VOUT, AOUT (Video and Audio Outputs)
4,14,25-32     I/O  CPU /NMI,/IRQ,D7,D6,D5,D4,D3,D2,D1,D0
5,24           Out  CPU A15, CIC 4MHz
6-10,38-42     I/O  Cart Pin 51-55,20-16
43,44,45       Out  OUT0, OUT1, OUT2 (Port 4016h Bit0-2 Outputs)
34 and 37      Out  PORT0-CLK (both pins) (CPU Read from Port 4016h)
11 and 17      Out  PORT1-CLK (both pins) (CPU Read from Port 4017h)
35,12,33,13,36 In   PORT0-0,1,2,3,4 (Port 4016h Bit0-3 Inverted Inputs)
19,20,15,16,18 In   PORT1-0,1,2,3,4 (Port 4017h Bit0-3 Inverted Inputs)
46             -    Unused
```
