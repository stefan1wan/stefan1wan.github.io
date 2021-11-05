---
layout: post
title: GDB Basic Commands
date: 2019-10-31
tags: Tutorial
author: Stefan Wan
---

# GDB--Basic Commands

Here is a simple tutorial of GDB I wrote before.
### GDB
+ gdb level1: use gab to debug binary level1
+ run: execute this binary
+ disas f_A: disassembly function f_A
+ break *0xdeedbeef: set a new breakpoint at position 0xdeedbeef
+ info breakpoint: check all the breakpoints
+ info register: check the states of registers 
+ x/wx  address: check the contents at address
    - w could be b/h/w/g as 1/2/4/8 bytes
    - x/100wx: show 100 four-byte word once a time
    - the second x could be u/d/s/x/w (determines the type of the memory address)
         * u: unsigned int
         * d: show as decimal number
         * x: show as hexadecimal number
         * s: show as strings
         * i: show as instructions
+ ni – finish next instruction(if it is a call, it will execute until it returns)
+ si - execute next instruction(if it is a call, it will execute the first instruction of the function next)
+ backtrace - show all the stack frames of the calling chain
+ continue - execute the process until it ends, crashes, or is at a breakpoint
+ set *address = value
     - set 4 bytes at address
     - change it to char, short, int, and long to represent 1,2, 4, and 8 bytes
     - e.g. set [int]0x80408000 = 666
     
+ attach [pid]: attach a running process

### programs compiled with debug symbols
+ list: list the source codes
+ b: add breakpoint at source code number
+ Info local: list local variables 
+ print val: print the value of variables 

### GDB-peda
+ checksec – check protection mechanisms of the binary 
+ elfsymbol - get every PLT addresses(useful at rop) 
+ vmmap -  check all the memory segments and permissions(read, write,execute) 
+ readelf -  check positions of important elf data structures(.plt, .plt.got, .bss)
+ find /bin/sh - find the address of string “/bin/sh”
