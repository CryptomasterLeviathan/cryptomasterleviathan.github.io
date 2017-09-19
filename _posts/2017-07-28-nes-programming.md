---
layout: post
title: "NES Programming"
categories: programming nes assembly
---

***This is still a work in progess!***  
These are my notes on creating a game for the NES. This document is meant to act as a quick reference when developing NES games. If you are new to developing on the NES I highly suggest reading bunnyboy's [Nerdy Nights Tutorial](http://nintendoage.com/forum/messageview.cfm?catid=22&threadid=7155).

## Table of Contents
{:.no_toc}
1. TOC
{:toc}


## To Do
- Research the correct headers for the code to run on actual NES hardware
- Use a standard syntax for describing command structure
- Overview of all opcodes and operands for the opcodes?
- Modify code snippets for clarity if needed (they are taken directly from tutorial currently)
- Add section about NMI interupt


## Unsorted Notes
- Accumulator (A): *TODO: Everything from mathematic operations to moving data?*
- Index Register X (X): Used for indexing in loops. Has specialized instructions for incrementing/decrementing
- Index Register Y (Y): *TODO: Differences from X. Can load values from Y to stack?*
- Status Register: Contains flags that can be set by the previous operation
- `#<value>` is used to indicate the number *value*. `<value>` represents the number stored in register *value*.

## Definitions
- Character data can also be called graphics data
- Binary values are prefixed with `%`
- Hexidecimal values are prefixed with `$`
- Directives are instructions for the assembler. These are executed when the binary file is being assembled and NOT by the NES CPU. Denoted by `[tab | two spaced | four spaces].<directive>`
- Opcodes are instructions that the CPU with execute. Denoted by `[tab | two spaced | four spaces]<instruction><operands (optional)>`
- Operands are additional information sent with the Opcodes
- Comments are notes for the developers describing what the code is doing. Comments are ignored and removed by the assembler. `<anything> ; <comment>`
- Labels are placeholders for memory addresses that will be determined and replaced by the assembler. `<anything excluding spaces>:`
- Register is an 8-bit *value?* stored inside the CPU and used to perform operations and indicate the status of the last operation
- Zero-Page Addressing: Memory addresses between $0000 and $00FF can be addressed with only the lower byte of the address. The processor will automatically add the higher byte $00. `LDA $EE     ; Load A with whatever value is in memory address $00EE.`

## Instruction Reference
Instead of recreating the intruction reference here I would highly suggest bookmarking Andrew Jacobs' [Instruction Reference](http://www.obelisk.me.uk/6502/reference.html).

## CPU Addresses

| Addresses     | Description                          |
|:-------------:|--------------------------------------|
| $0000-0800    | Internal RAM, 2KB chip in the NES    |
| $2000-2007    | PPU access ports                     |
| $4000-4017    | Audio and controller access ports    |
| $6000-7FFF    | Optional WRAM inside the game cart   |
| $8000-FFFF    | Game cart ROM *Can be bank switched* |

*TODO: Add more to description about the actual usage of these addresses*

## PPU Addresses

| Addresses     | Description |
|:-------------:| ----------- |
| $0000 - $0800 | Test test t |

## CPU Hardware Limitations
- 2KB of RAM on the motherboard

## PPU Hardware Limitations
- *TODO: 8?* sprites per scan line
- 2KB of RAM on the PPU for storing two screens of background graphics
    - Some cartidges have 4KB of PPU RAM to store four screens of background graphics
- The CHR chip can be RAM or ROM
    - If the CHR chip is RAM then graphics data can be copied from PGR to CHR *TODO: why?*
- The PPU can hold up to 64 sprites
  - Each sprite is 8px by 8px
    - Larger characters or background images must be made up of multiple sprites
  - Sprites are image data that can be used for backgrounds or characters

## Horizontal Scrolling
- *TODO: Sprite0 collisions*

## Game Development
- Collision are detected in software

## Architecture Basics
- ROM cannot be changed, usually contains the program code and character (graphics) data
- RAM can be changed, usually contains the variables used by the code
- RAM contents will be lost when the power is removed unless battery backed
- The NES has 8-bit registers
- The NES has a 16-bit address bus
    - Two 8-bit registers must be used to address memory
- ROM is where the game code (PRG) and the character/graphics data (CHR) is stored

## Game Cartridge
- The game cart always contains PRG ROM
- The game cart must also contain CHR ROM or more PRG RAM
- Some  game carts contain WRAM (Work RAM) is battery backed RAM

## Graphics
### Palettes
- There are two 16 byte (holds 16 colors) pallets that must be set in the NES
  - One is for the sprites and one is for the background
  - The palettes start at $3F00 and $3F10
  - Set the values in the pallets using address $2006

The following code demonstrates getting the PPU ready to set the pallet colors at $3F00
```
  LDA $2002    ; read PPU status to reset the high/low latch to high
  LDA #$3F
  STA $2006    ; write the high byte of $3F10 address
  LDA #$00
  STA $2006    ; write the low byte of $3F10 address
```
Now, storing a value in $2007 will cause the PPU to add the value to the pallet (at $3F00 as seen in the code). The PPU will automatically increment the pallete address when a value is written. That means that the next value stored in $2007 will be placed in the pallet at $3F01. A `.db` directive can be used to store multiple bytes of data and a loop can write the data to $2007 one byte at a time.
```
PaletteData:
  .db $0F,$31,$32,$33,$0F,$35,$36,$37,$0F,$39,$3A,$3B,$0F,$3D,$3E,$0F  ;background palette data
  .db $0F,$1C,$15,$14,$0F,$02,$38,$3C,$0F,$1C,$15,$14,$0F,$02,$38,$3C  ;sprite palette data
```
```
  LDX #$00                ; start out at 0
LoadPalettesLoop:
  LDA PaletteData, x      ; load data from address (PaletteData + the value in x)
                          ; 1st time through loop it will load PaletteData+0
                          ; 2nd time through loop it will load PaletteData+1
                          ; 3rd time through loop it will load PaletteData+2
                          ; etc
  STA $2007               ; write to PPU
  INX                     ; X = X + 1
  CPX #$20                ; Compare X to hex $20, decimal 32
  BNE LoadPalettesLoop    ; Branch to LoadPalettesLoop if compare was Not Equal to zero
                          ; if compare was equal to 32, keep going down
```

## Resources
- bunnyboy's [Nerdy Nights Tutorial](http://nintendoage.com/forum/messageview.cfm?catid=22&threadid=7155)
- Andrew Jacobs' [Instruction Reference](http://www.obelisk.me.uk/6502/reference.html)
- [NES Hacker](http://www.thealmightyguru.com/Games/Hacking/Wiki/index.php)
- [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)