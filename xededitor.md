# Xededitor

> Source: https://problemkaputt.de/everynes.htm
> Section: Xededitor

XED Editor
[XED About](#xedabout)

[XED Hotkeys](#xedhotkeys)

[XED Assembler/Debugger Interface](#xedassemblerdebuggerinterface)

[XED Commandline based standalone version](#xedcommandlinebasedstandaloneversion)

## XED Hotkeys

XED recognizes both CP/M Wordstar hotkeys (also used by Borland PC compilers),
and Norton editor hotkeys (NU.EXE). The "Ctrl+X,Y" style hotkeys are wordstar
based, particulary including good block functions. The F4,X and Alt/Ctrl+X type
hotkeys are norton based, particulary very useful for forwards/backwards
searching.

**Standard Cursor Keys**

```
Up         Move line up
Down       Move line down
Left       Move character left
Right      Move character right
Pgup       Scroll page up / to top of screen
Pgdn       Scroll page down / to bottom of screen
Ctrl+Pgup  Go to start of file (or Ctrl+Home)
Ctrl+Pgdn  Go to end of file (or Ctrl+End)
Home       Go to start of line
End        Go to end of line
Ctrl+Left  Move word left
Ctrl+Right Move word right
Ins        Toggle Insert/Overwrite mode
Del        Delete char below cursor
Backspace  Delete char left of cursor
Tab        Move to next tabulation mark
Enter      New line/paragraph end
Esc        Quit (or Alt+X, F3+Q, Ctrl+K+D, Ctrl+K+Q, Ctrl+K+X)
```

Note: Pgup/Pgdn are moving the cursor to top/bottom of screen, page scrolling
takes place only if the cursor was already at that location.

**Editor Keys**

```
Ctrl+Y     Delete line (or Alt+K)
Alt+L      Delete to line end (or Ctrl+Q,Y)
Alt+V      Caseflip to line end
Ctrl+V     Caseflip from line beginning
```

**Norton Search/Replace Functions**

```
Alt+F      Norton - search/replace, forwards
Ctrl+F     Norton - search/replace, backwards
Alt+C      Norton - continue search/replace, forwards
Ctrl+C     Norton - continue search/replace, backwards
```

Search: Type "Alt/Ctrl+F, String, Enter".

Search+replace: "Type Alt/Ctrl+F, String1, Alt+F, String2, Enter".

Non-case sensitive: Terminate by Escape instead of Enter.

**Wordstar Search/Replace Functions**

```
Ctrl+Q,F   Wordstar - search
Ctrl+Q,A   Wordstar - replace
Ctrl+L     Wordstar - continue search/replace
```

Search options: B=Backwards, G=Global, N=No query,

U=non-casesensitive, W=whole words only, n=n times.

**Disk Commands**

```
F3,E       Save+exit
F3,S       Save (or Ctrl+K,S)
F3,N       Edit new file
F3,A       Append a file
```

See also: Block commands (read/write block).

**Block Selection**

```
Shift+Cursor  Select block begin..end
Ctrl+K,B   Set block begin (or F4,S)
Ctrl+K,K   Set block end   (or F4,S)
Ctrl+K,H   Remove/hide block markers (or F4,R)
F4,L       Mark line including ending CRLF (or Ctrl+K,L)
F4,E       Mark line excluding ending CRLF
Ctrl+K,T   Mark word
Ctrl+K,N   Toggle normal/column blocktype
```

**Clipboard Commands**

```
Shift+Ins  Paste from Clipboard
Shift+Del  Cut to Clipboard
Ctrl+Ins   Copy to Clipboard
Ctrl+Del   Delete Block
```

**Block Commands**

```
Ctrl+K,C   Copy block (or F4,C)
Ctrl+K,V   Move block (or F4,M)
Ctrl+K,Y   Delete block (or F4,D)
Ctrl+K,P   Print block  (or F7,B)
Ctrl+Q,B   Find block begin (or F4,F)
Ctrl+Q,K   Find block end   (or F4,F)
Ctrl+K,R   Read block from disk towards cursor location
Ctrl+K,W   Write block to disk
Ctrl+K,U   Unindent block (delete one space at begin of each line)
Ctrl+K,I   Indent block (insert one space at begin of each line)
F5,F       Format block (with actual x-wrap size) (or ;Ctrl+B)
F8,A       Add values within column-block
```

**Setup Commands**

```
F11        Setup menu (or F8,S)
F5,S       Save editor configuration
F5,L       Set line len for word wrap (or Ctrl+O,R)
F5,W       Wordwrap on/off (or Ctrl+O,W) (*)
F5,I       Auto indent on/off (or Ctrl+O,I)
F5,T       Set tab display spacing
```

(*) Wrapped lines will be terminated by CR, paragraphs by CRLF.

**Other**

```
F1         Help
F2         Status (displays info about file & currently selected block)
F8,M       Make best fill tabs
F8,T       Translate all tabs to spaces
SrcLock    Freeze cursor when typing text ("useful" for backwards writing)
Ctrl+O,C   Center current line
Ctrl+K,#   Set marker (#=0..9)
Ctrl+Q,#   Move to marker (#=0..9)
Ctrl+Q,P   Move to previous pos
F6,C       Condensed display mode on/off (*)
Ctrl+G     Go to line nnnn (or F6,G) (or commandline switch /l:nnnn)
```

(*) only lines starting with ':' or ';:' will be displayed. cursor and block
commands can be used (e.g. to copy a text-sequence by just marking it's
headline)

**Hex-Edit Keys (Binary Files)**

This mode is activated by /b commandline switch, allowing to view and modify
binary files. Aside from normal cursor keys, the following hotkeys are used:

```
Tab        Toggle between HEX and ASC mode (or Shift+Left/Right)
Ctrl+Arrow Step left/right one full byte (instead one single HEX digit)
Ctrl+G     Goto hex-address
Ctrl+K,S   Save file (as usually)
```

**Printer Commands**

```
F7,P       Print file
F7,B       Print block (or Ctrl+K,P)
F7,E       Eject page
F7,S       Set page size
```

More printer options can be found in setup. Printing was working well (at least
with my own printer) in older XED versions, but it is probably badly bugged (at
least untested) for years now.

## XED Commandline based standalone version

**Standalone 16bit DOS version**

This version is written in turbo pascal, nevertheless fast enough to work on
computer with less than 10MHz. It uses 16bit 8086 code, and works with all
80x86 compatible CPUs, including very old XTs.

The downside is that it is restricted to Conventional DOS Memory, so that the
maximum filesize is 640K (actually less, because the program and operating
system need to use some of that memory).

**Using the 32bit debugger-built-in version as 32bit standalone editor**

I haven't yet compiled a 32bit standalone version, however, any of the no$xxx
32bit debuggers can be used for that purpose. By commandline input:

```
no$xxx /x    Edit text file in standalone mode
no$xxx /b    Edit binary file in standalone hexedit mode
```

**Standalone Commandline Syntax**

Syntax: XED <filename> [/l:<line number>] | /?

```
Filename, optionally d:\path\name.ext
/?        Displays commandline help
/l:  Moves to line number nnn after loading
```

The filename does not require to include an extension, the program
automatically loads the first existing file with any of following extensions
appended: XED, ASM, ASC, INC, BAT, TXT, HTM, DOC, A22, PAS.

**Standalone DOS Return Value**

XED returns a three-byte return value after closing the program. This data is
used when calling XED as external editor from inside of nocash DOS debuggers,
but it might be also useful for other purposes.

Because normal DOS return values are limited to 8bits only, the three bytes are
written into video RAM at rightmost three character locations in first line:

```
VSEG:77*2  Exit code   (00h=Exit normal, F9h=Exit by F9-key)
VSEG:78*2  Line number (Lower 8bits, 1..65536 in total)
VSEG:79*2  Line number (Upper 8bits)
```

The color attribute for these characters is set to zero (invisible, black on
black). Use INT 10h/AH=0Fh to determine the current video mode (AL AND 7Fh), if
it is monochrome (07h) then use VSEG=B000h, otherwise VSEG=B800h.
