# Iomap

> Source: https://problemkaputt.de/everynes.htm
> Section: Iomap

I/O Map 
**I/O Map**

```
2000h - PPU Control Register 1 (W)
2001h - PPU Control Register 2 (W)
2002h - PPU Status Register (R)
2003h - SPR-RAM Address Register (W)
2004h - SPR-RAM Data Register (RW)
2005h - PPU Background Scrolling Offset (W2)
2006h - VRAM Address Register (W2)
2007h - VRAM Read/Write Data Register (RW)
4000h - APU Channel 1 (Rectangle) Volume/Decay (W)
4001h - APU Channel 1 (Rectangle) Sweep (W)
4002h - APU Channel 1 (Rectangle) Frequency (W)
4003h - APU Channel 1 (Rectangle) Length (W)
4004h - APU Channel 2 (Rectangle) Volume/Decay (W)
4005h - APU Channel 2 (Rectangle) Sweep (W)
4006h - APU Channel 2 (Rectangle) Frequency (W)
4007h - APU Channel 2 (Rectangle) Length (W)
4008h - APU Channel 3 (Triangle) Linear Counter (W)
4009h - APU Channel 3 (Triangle) N/A (-)
400Ah - APU Channel 3 (Triangle) Frequency (W)
400Bh - APU Channel 3 (Triangle) Length (W)
400Ch - APU Channel 4 (Noise) Volume/Decay (W)
400Dh - APU Channel 4 (Noise) N/A (-)
400Eh - APU Channel 4 (Noise) Frequency (W)
400Fh - APU Channel 4 (Noise) Length (W)
4010h - APU Channel 5 (DMC) Play mode and DMA frequency (W)
4011h - APU Channel 5 (DMC) Delta counter load register (W)
4012h - APU Channel 5 (DMC) Address load register (W)
4013h - APU Channel 5 (DMC) Length register (W)
4014h - SPR-RAM DMA Register (W)
4015h - DMC/IRQ/length counter status/channel enable register (RW)
4016h - Joypad #1 (RW)
4017h - Joypad #2/APU SOFTCLK (RW)
```

Additionally, external hardware may contain further ports:

```
4020h - VS Unisystem Coin Acknowledge
4020h-40FFh - Famicom Disk System (FDS)
4100h-FFFFh - Various addresses used by various cartridge mappers
```
