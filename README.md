# OS_kernel – A Learning-Based Multithreaded Kernel Project

This repository contains `OS_kernel`, a **learning-oriented reimplementation** of a multitasking operating system kernel, based on the [PeachOS](https://github.com/nibblebits/PeachOS) project by nibblebits.

The primary goal is to **deeply understand operating system internals** by rebuilding a similar kernel from scratch, covering topics like bootloading, protected mode, memory management, multitasking, and more.

---

##  Project Objectives

- Learn the low-level structure of an operating system kernel
- Gain hands-on experience in **x86 architecture**, **real/protected mode**, and **task scheduling**
- Reinforce C and x86 Assembly programming in a systems-level context
- Practice debugging OS-level code with QEMU and GDB

---

##  What's Implemented

### Real Mode Bootloader
- Writing a custom bootloader in assembly
- Setting up segment registers
- Reading disk sectors using BIOS interrupts

###  Protected Mode Kernel
- Entering 32-bit protected mode using GDT
- Interrupt Descriptor Table (IDT) setup
- Paging and virtual memory
- FAT16 file system support
- ELF file loading and process spawning
- Cooperative multitasking and context switching

###  Devices and Drivers
- PS/2 Keyboard input handling
- Simple terminal interface

---

##  Educational Focus

> This is a **self-study project**, aiming to **recreate** core elements of the [PeachOS](https://github.com/nibblebits/PeachOS) kernel in order to understand how operating systems work from the ground up.

While the code is written independently, much of the architecture and implementation logic follows the design presented in PeachOS.

---

## Project Reference

This project is based on and heavily inspired by:  
**[PeachOS by nibblebits](https://github.com/nibblebits/PeachOS)**  
Special thanks to the original author for creating such an excellent educational OS kernel tutorial.

