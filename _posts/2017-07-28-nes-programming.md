---
layout: post
title: "NES Programming"
categories: programming nes assembly
---

***This is still a work in progess!***
These are my notes on creating a game for the NES.

## Table of Contents
{:.no_toc}
1. TOC
{:toc}

## Definitions
- Character data can also be called graphics data
- Binary values are prefixed with %
- Hexidecimal values are prefixed with $

## Hardware Limitations
- *TODO: 8?* sprites per scan line
- 2KB of RAM on the motherboard
- 2KB of RAM on the PPU for storing two screens of background graphics
    - Some cartidges have 4KB of PPU RAM to store four screens of background graphics
- The CHR chip can be RAM or ROM
    - If the CHR chip is RAM then graphics data can be copied from PGR to CHR *TODO: why?*

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
- [Nerdy Nights Tutorials](http://nintendoage.com/forum/messageview.cfm?catid=22&threadid=7155)