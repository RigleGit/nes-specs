# Memorymaps

> Source: https://problemkaputt.de/everynes.htm
> Section: Memorymaps

Memory Maps
Internal Memory: 2K WRAM, 2K VRAM, 256 Bytes SPR-RAM, and Palette/Registers

**CPU Memory Map (16bit buswidth, 0-FFFFh)**

```
0000h-07FFh   Internal 2K Work RAM (mirrored to 800h-1FFFh)
2000h-2007h   Internal PPU Registers (mirrored to 2008h-3FFFh)
4000h-4017h   Internal APU Registers
4018h-5FFFh   Cartridge Expansion Area almost 8K
6000h-7FFFh   Cartridge SRAM Area 8K
8000h-FFFFh   Cartridge PRG-ROM Area 32K
```

CPU Reset vector located at [FFFC], even smaller carts must have memory at that
location. Larger carts may use whatever external mappers to access more than
the usual 32K.

**PPU Memory Map (14bit buswidth, 0-3FFFh)**

```
0000h-0FFFh   Pattern Table 0 (4K) (256 Tiles)
1000h-1FFFh   Pattern Table 1 (4K) (256 Tiles)
2000h-23FFh   Name Table 0 and Attribute Table 0 (1K) (32x30 BG Map)
2400h-27FFh   Name Table 1 and Attribute Table 1 (1K) (32x30 BG Map)
2800h-2BFFh   Name Table 2 and Attribute Table 2 (1K) (32x30 BG Map)
2C00h-2FFFh   Name Table 3 and Attribute Table 3 (1K) (32x30 BG Map)
3000h-3EFFh   Mirror of 2000h-2EFFh
3F00h-3F1Fh   Background and Sprite Palettes (25 entries used)
3F20h-3FFFh   Mirrors of 3F00h-3F1Fh
```

Note: The NES contains only 2K built-in VRAM, which can be used for whatever
purpose (for example, as two Name Tables, or as one Name Table plus 64 Tiles).
Palette Memory is built-in as well. Any additional VRAM (or, more regulary,
VROM) is located in the cartridge, which may also contain mapping hardware to
access more than 12K of video memory.

**SPR-RAM Memory Map (8bit buswidth, 0-FFh)**

```
00-FF         Sprite Attributes (256 bytes, for 64 sprites / 4 bytes each)
```

Sprite RAM is directly built-in in the PPU chip. SPR-RAM is not connected to
CPU or PPU bus, and can be accessed via I/O Ports only.
