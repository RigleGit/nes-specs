# Cpu65xxmicroprocessor

> Source: https://problemkaputt.de/everynes.htm
> Section: Cpu65xxmicroprocessor

CPU 65XX Microprocessor 
**Overview**

CPU Registers and Flags

CPU Memory Addressing

**Instruction Set**

CPU Memory and Register Transfers

CPU Arithmetic/Logical Operations

CPU Rotate and Shift Instructions

CPU Jump and Control Instructions

CPU Illegal Opcodes

**Other Info**

CPU Assembler Directives/Syntax

CPU Glitches

CPU The 65XX Family

CPU Local Usage

CPU Memory Addressing

**Opcode Addressing Modes**

```
Name           Native   Nocash
Implied        -        A,X,Y,S,P
Immediate      #nn      nn
Zero Page      nn       [nn]
Zero Page,X    nn,X     [nn+X]
Zero Page,Y    nn,Y     [nn+Y]
Absolute       nnnn     [nnnn]
Absolute,X     nnnn,X   [nnnn+X]
Absolute,Y     nnnn,Y   [nnnn+Y]
(Indirect,X)   (nn,X)   [[nn+X]]
(Indirect),Y   (nn),Y   [[nn]+Y]
```

**Zero Page - [nn] [nn+X] [nn+Y]**

Uses an 8bit parameter (one byte) to address the first 256 of memory at
0000h..00FFh. This limited range is used even for "nn+X" and "nn+Y", ie.
"C0h+60h" will access 0020h (not 0120h).

**Absolute - [nnnn] [nnnn+X] [nnnn+Y]**

Uses a 16bit parameter (two bytes) to address the whole 64K of memory at
0000h..FFFFh. Because of the additional parameter bytes, this is a bit slower
than Zero Page accesses.

**Indirect - [[nn+X]] [[nn]+Y]**

Uses an 8bit parameter that points to a 16bit parameter in page zero.

Even though the CPU doesn't support 16bit registers (except for the program
counter), this (double-)indirect addressing mode allows to use variable 16bit
pointers.

**On-Chip Bi-directional I/O port**

Addresses (00)00h and (00)01h are occupied by an I/O port which is built-in
into 6510, 8500, 7501, 8501 CPUs (eg. used in C64 and C16), be sure not to use
the addresses as normal memory. For description read chapter about I/O ports.

**Caution**

Because of the identical format, assemblers will be more or less unable to
separate between [XXh+r] and [00XXh+r], the assembler will most likely produce
[XXh+r] when address is already known to be located in page 0, and [00XXh+r] in
case of forward references.

Beside for different opcode size/time, [XXh+r] will always access page 0 memory
(even when XXh+r>FFh), while [00XXh+r] may direct to memory in page 0 or 1,
to avoid unpredictable results be sure not to use (00)XXh+r>FFh if possible.

CPU Arithmetic/Logical Operations

**Add memory to accumulator with carry**

```
69 nn     nzc--v  2   ADC #nn     ADC A,nn            ;A=A+C+nn
65 nn     nzc--v  3   ADC nn      ADC A,[nn]          ;A=A+C+[nn]
75 nn     nzc--v  4   ADC nn,X    ADC A,[nn+X]        ;A=A+C+[nn+X]
6D nn nn  nzc--v  4   ADC nnnn    ADC A,[nnnn]        ;A=A+C+[nnnn]
7D nn nn  nzc--v  4*  ADC nnnn,X  ADC A,[nnnn+X]      ;A=A+C+[nnnn+X]
79 nn nn  nzc--v  4*  ADC nnnn,Y  ADC A,[nnnn+Y]      ;A=A+C+[nnnn+Y]
61 nn     nzc--v  6   ADC (nn,X)  ADC A,[[nn+X]]      ;A=A+C+[word[nn+X]]
71 nn     nzc--v  5*  ADC (nn),Y  ADC A,[[nn]+Y]      ;A=A+C+[word[nn]+Y]
```

* Add one cycle if indexing crosses a page boundary.

**Subtract memory from accumulator with borrow**

```
E9 nn     nzc--v  2   SBC #nn     SBC A,nn            ;A=A+C-1-nn
E5 nn     nzc--v  3   SBC nn      SBC A,[nn]          ;A=A+C-1-[nn]
F5 nn     nzc--v  4   SBC nn,X    SBC A,[nn+X]        ;A=A+C-1-[nn+X]
ED nn nn  nzc--v  4   SBC nnnn    SBC A,[nnnn]        ;A=A+C-1-[nnnn]
FD nn nn  nzc--v  4*  SBC nnnn,X  SBC A,[nnnn+X]      ;A=A+C-1-[nnnn+X]
F9 nn nn  nzc--v  4*  SBC nnnn,Y  SBC A,[nnnn+Y]      ;A=A+C-1-[nnnn+Y]
E1 nn     nzc--v  6   SBC (nn,X)  SBC A,[[nn+X]]      ;A=A+C-1-[word[nn+X]]
F1 nn     nzc--v  5*  SBC (nn),Y  SBC A,[[nn]+Y]      ;A=A+C-1-[word[nn]+Y]
```

* Add one cycle if indexing crosses a page boundary.

Note: Compared with normal 80x86 and Z80 CPUs, incoming and resulting Carry
Flag are reversed.

**Logical AND memory with accumulator**

```
29 nn     nz----  2   AND #nn     AND A,nn            ;A=A AND nn
25 nn     nz----  3   AND nn      AND A,[nn]          ;A=A AND [nn]
35 nn     nz----  4   AND nn,X    AND A,[nn+X]        ;A=A AND [nn+X]
2D nn nn  nz----  4   AND nnnn    AND A,[nnnn]        ;A=A AND [nnnn]
3D nn nn  nz----  4*  AND nnnn,X  AND A,[nnnn+X]      ;A=A AND [nnnn+X]
39 nn nn  nz----  4*  AND nnnn,Y  AND A,[nnnn+Y]      ;A=A AND [nnnn+Y]
21 nn     nz----  6   AND (nn,X)  AND A,[[nn+X]]      ;A=A AND [word[nn+X]]
31 nn     nz----  5*  AND (nn),Y  AND A,[[nn]+Y]      ;A=A AND [word[nn]+Y]
```

* Add one cycle if indexing crosses a page boundary.

**Exclusive-OR memory with accumulator**

```
49 nn     nz----  2   EOR #nn     XOR A,nn            ;A=A XOR nn
45 nn     nz----  3   EOR nn      XOR A,[nn]          ;A=A XOR [nn]
55 nn     nz----  4   EOR nn,X    XOR A,[nn+X]        ;A=A XOR [nn+X]
4D nn nn  nz----  4   EOR nnnn    XOR A,[nnnn]        ;A=A XOR [nnnn]
5D nn nn  nz----  4*  EOR nnnn,X  XOR A,[nnnn+X]      ;A=A XOR [nnnn+X]
59 nn nn  nz----  4*  EOR nnnn,Y  XOR A,[nnnn+Y]      ;A=A XOR [nnnn+Y]
41 nn     nz----  6   EOR (nn,X)  XOR A,[[nn+X]]      ;A=A XOR [word[nn+X]]
51 nn     nz----  5*  EOR (nn),Y  XOR A,[[nn]+Y]      ;A=A XOR [word[nn]+Y]
```

* Add one cycle if indexing crosses a page boundary.

**Logical OR memory with accumulator**

```
09 nn     nz----  2   ORA #nn     OR  A,nn            ;A=A OR nn
05 nn     nz----  3   ORA nn      OR  A,[nn]          ;A=A OR [nn]
15 nn     nz----  4   ORA nn,X    OR  A,[nn+X]        ;A=A OR [nn+X]
0D nn nn  nz----  4   ORA nnnn    OR  A,[nnnn]        ;A=A OR [nnnn]
1D nn nn  nz----  4*  ORA nnnn,X  OR  A,[nnnn+X]      ;A=A OR [nnnn+X]
19 nn nn  nz----  4*  ORA nnnn,Y  OR  A,[nnnn+Y]      ;A=A OR [nnnn+Y]
01 nn     nz----  6   ORA (nn,X)  OR  A,[[nn+X]]      ;A=A OR [word[nn+X]]
11 nn     nz----  5*  ORA (nn),Y  OR  A,[[nn]+Y]      ;A=A OR [word[nn]+Y]
```

* Add one cycle if indexing crosses a page boundary.

**Compare**

```
C9 nn     nzc---  2   CMP #nn     CMP A,nn            ;A-nn
C5 nn     nzc---  3   CMP nn      CMP A,[nn]          ;A-[nn]
D5 nn     nzc---  4   CMP nn,X    CMP A,[nn+X]        ;A-[nn+X]
CD nn nn  nzc---  4   CMP nnnn    CMP A,[nnnn]        ;A-[nnnn]
DD nn nn  nzc---  4*  CMP nnnn,X  CMP A,[nnnn+X]      ;A-[nnnn+X]
D9 nn nn  nzc---  4*  CMP nnnn,Y  CMP A,[nnnn+Y]      ;A-[nnnn+Y]
C1 nn     nzc---  6   CMP (nn,X)  CMP A,[[nn+X]]      ;A-[word[nn+X]]
D1 nn     nzc---  5*  CMP (nn),Y  CMP A,[[nn]+Y]      ;A-[word[nn]+Y]
E0 nn     nzc---  2   CPX #nn     CMP X,nn            ;X-nn
E4 nn     nzc---  3   CPX nn      CMP X,[nn]          ;X-[nn]
EC nn nn  nzc---  4   CPX nnnn    CMP X,[nnnn]        ;X-[nnnn]
C0 nn     nzc---  2   CPY #nn     CMP Y,nn            ;Y-nn
C4 nn     nzc---  3   CPY nn      CMP Y,[nn]          ;Y-[nn]
CC nn nn  nzc---  4   CPY nnnn    CMP Y,[nnnn]        ;Y-[nnnn]
```

* Add one cycle if indexing crosses a page boundary.

Note: Compared with normal 80x86 and Z80 CPUs, resulting Carry Flag is
reversed.

**Bit Test**

```
24 nn     xz---x  3   BIT nn      TEST A,[nn]         ;test and set flags
2C nn nn  xz---x  4   BIT nnnn    TEST A,[nnnn]       ;test and set flags
```

Flags are set as so: Z=((A AND [addr])=00h), N=[addr].Bit7, V=[addr].Bit6. Note
that N and V are affected only by [addr] (not by A).

**Increment by one**

```
E6 nn     nz----  5   INC nn      INC [nn]            ;[nn]=[nn]+1
F6 nn     nz----  6   INC nn,X    INC [nn+X]          ;[nn+X]=[nn+X]+1
EE nn nn  nz----  6   INC nnnn    INC [nnnn]          ;[nnnn]=[nnnn]+1
FE nn nn  nz----  7   INC nnnn,X  INC [nnnn+X]        ;[nnnn+X]=[nnnn+X]+1
E8        nz----  2   INX         INC X               ;X=X+1
C8        nz----  2   INY         INC Y               ;Y=Y+1
```

**Decrement by one**

```
C6 nn     nz----  5   DEC nn      DEC [nn]            ;[nn]=[nn]-1
D6 nn     nz----  6   DEC nn,X    DEC [nn+X]          ;[nn+X]=[nn+X]-1
CE nn nn  nz----  6   DEC nnnn    DEC [nnnn]          ;[nnnn]=[nnnn]-1
DE nn nn  nz----  7   DEC nnnn,X  DEC [nnnn+X]        ;[nnnn+X]=[nnnn+X]-1
CA        nz----  2   DEX         DEC X               ;X=X-1
88        nz----  2   DEY         DEC Y               ;Y=Y-1
```

CPU Jump and Control Instructions

**Normal Jumps & Subroutine Calls/Returns**

```
4C nn nn  ------  3   JMP nnnn     JMP nnnn                 ;PC=nnnn
6C nn nn  ------  5   JMP (nnnn)   JMP [nnnn]               ;PC=WORD[nnnn]
20 nn nn  ------  6   JSR nnnn     CALL nnnn                ;[S]=PC+2,PC=nnnn
40        nzcidv  6   RTI          RETI ;(from BRK/IRQ/NMI) ;P=[S], PC=[S]
60        ------  6   RTS          RET  ;(from CALL)        ;PC=[S]+1
```

Note: RTI cannot modify the B-Flag or the unused flag.

Glitch: For JMP [nnnn] the operand word cannot cross page boundaries, ie. JMP
[03FFh] would fetch the MSB from [0300h] instead of [0400h]. Very simple
workaround would be to place a ALIGN 2 before the data word.

**Conditional Branches (conditional jump to PC=PC+/-dd)**

```
10 dd     ------  2** BPL nnn      JNS nnn     ;N=0 plus/positive
30 dd     ------  2** BMI nnn      JS  nnn     ;N=1 minus/negative/signed
50 dd     ------  2** BVC nnn      JNO nnn     ;V=0 no overflow
70 dd     ------  2** BVS nnn      JO  nnn     ;V=1 overflow
90 dd     ------  2** BCC/BLT nnn  JNC/JB  nnn ;C=0 less/below/no carry
B0 dd     ------  2** BCS/BGE nnn  JC/JAE  nnn ;C=1 above/greater/equal/carry
D0 dd     ------  2** BNE/BZC nnn  JNZ/JNE nnn ;Z=0 not zero/not equal
F0 dd     ------  2** BEQ/BZS nnn  JZ/JE   nnn ;Z=1 zero/equal
```

** The execution time is 2 cycles if the condition is false (no branch
executed). Otherwise, 3 cycles if the destination is in the same memory page,
or 4 cycles if it crosses a page boundary (see below for exact info).

Note: After subtractions (SBC or CMP) carry=set indicates above-or-equal,
unlike as for 80x86 and Z80 CPUs.

**Interrupts, Exceptions, Breakpoints**

```
00        ---1--  7   BRK   Force Break B=1,[S]=PC+1,[S]=P,I=1,PC=[FFFE]
--        ---1--  7   /IRQ  Interrupt   B=0,[S]=PC,  [S]=P,I=1,PC=[FFFE]
--        ---1--  7   /NMI  NMI         B=0,[S]=PC,  [S]=P,I=1,PC=[FFFA]
--        ---1-- T+6? /RESET Reset      B=1,S=S-3,         I=1,PC=[FFFC]
```

Notes: IRQs can be disabled by setting the I-flag. BRK command, /NMI signal,
and /RESET signal cannot be masked by setting I.

BRK/IRQ/NMI first change the B-flag, then write P to stack, and then set the
I-flag, the D-flag is NOT changed and should be cleared by software.

The same vector is shared for BRK and IRQ, software can separate between BRK
and IRQ by examining the pushed B-flag only.

The RTI opcode can be used to return from BRK/IRQ/NMI, note that using the
return address from BRK skips one dummy/parameter byte following after the BRK
opcode.

Software or hardware must take care to acknowledge or reset /IRQ or /NMI
signals after processing it.

```
IRQs are executed whenever "/IRQ=LOW AND I=0".
NMIs are executed whenever "/NMI changes from HIGH to LOW".
```

If /IRQ is kept LOW then same (old) interrupt is executed again as soon as
setting I=0. If /NMI is kept LOW then no further NMIs can be executed.

**CPU Control**

```
18        --0---  2   CLC       CLC    ;Clear carry flag            C=0
58        ---0--  2   CLI       EI     ;Clear interrupt disable bit I=0
D8        ----0-  2   CLD       CLD    ;Clear decimal mode          D=0
B8        -----0  2   CLV       CLV    ;Clear overflow flag         V=0
38        --1---  2   SEC       STC    ;Set carry flag              C=1
78        ---1--  2   SEI       DI     ;Set interrupt disable bit   I=1
F8        ----1-  2   SED       STD    ;Set decimal mode            D=1
```

**No Operation**

```
EA        ------  2   NOP       NOP    ;No operation
```

**Conditional Branch Page Crossing**

The branch opcode with parameter takes up two bytes, causing the PC to get
incremented twice (PC=PC+2), without any extra boundary cycle. The signed
parameter is then added to the PC (PC+disp), the extra clock cycle occurs if
the addition crosses a page boundary (next or previous 100h-page).

CPU Assembler Directives/Syntax

Below are some common 65XX assembler directives, and the corresponding
expressions in 80XX-style language.

```
**65XX-style    80XX-style         Expl.**
.native       .nocash            select native or nocash syntax
*=$c100       org 0c100h         sets the assumed origin in memory
*=*+8         org $+8            increments origin, does NOT produce data
label         label:             sets a label equal to the current address
label=$dc00   label equ 0dc00h   assigns a value or address to label
.by $00       db 00h             defines a (list of) byte(s) in memory
.byt $00      defb 00h           same as .by and db
.wd $0000     dw 0000h           defines a (list of) word(s) in memory
.end          end                indicates end of source code file
|nn           [|nn]              force 16bit "00NN" instead 8bit "NN"
#nnnn        nnnn DIV 100h      isolate upper 8bits of 16bit value
N/A (?)       fast label         ensure relative jump without page crossing
N/A (?)       slow label         ensure relative jump with page crossing
```

**Special Directives**

```
.65xx       Select 6502 Instruction Set
.nes        Create NES ROM-Image with .NES extension
.c64_prg    Create C64 file with .PRG extension/stub/fixed entry
.c64_p00    Create C64 file with .P00 extension/stub/fixed entry/header
.vic20_prg  Create VIC20/C64 file with .PRG extension/stub/relocated entry
end entry   End of Source, the parameter specifies the entrypoint
```

The C64 files contain Basic Stub "10 SYS<entry>" with default ORG 80Eh.

**VIC20 Stub**

The VIC20 Stub is "10 SYSPEEK(44)*256+<entry>" with default ORG 1218h,
this relocates the entryoint relative to the LOAD address (for C64: 818h, for
VIC20: 1018h (Unexpanded), 0418h (3K Expanded), 1218h (8K and more Expansion).
It does NOT relocate absolute addresses in the program, if the program wishes
to run at a specific memory location, then it must de-relocate itself from the
LOAD address to the desired address.

CPU The 65XX Family

Different versions of the 6502:

All of these processors are the same concerning the software-side:

```
6501   Some sort of 6502 prototype
6502   Used in the CBM floppies and some other 8 bit computers.
6507   Used in Atari 2600, 28pins (only 13 address lines, no /IRQ, no /NMI).
6510   Used in C64, with built-in 6bit I/O port.
7501   Used in C16,C116,Plus/4, with built-in 7bit I/O Port, without /NMI pin.
8500   Used in C64-II, with different pin-outs.
8501   Same as 7501
8502   Used in C128s.
```

Some processors of the family which are not 100% compatible:

```
65C02  Extension of the 6502
65SC02 Small version of the 65C02 which lost a few opcodes again.
65CE02 Extension of the 65C02, used in the C65.
65816  Extended 6502 with new opcodes and 16 bit operation modes.
2A03   Nintendo NES/Famicom, modified 6502 with built-in sound controller.
```
