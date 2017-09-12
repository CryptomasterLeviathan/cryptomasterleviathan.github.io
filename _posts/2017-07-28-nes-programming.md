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
- Use a standard syntax for describing command structure
- Overview of all opcodes and operands for the opcodes?


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

## Resources
- bunnyboy's [Nerdy Nights Tutorial](http://nintendoage.com/forum/messageview.cfm?catid=22&threadid=7155)
- [Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)