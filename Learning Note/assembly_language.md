# 📘 8086 Assembly Language Quick Reference

This document summarizes key registers, instructions, and essential concepts used in 8086 assembly programming, with a focus on real-mode applications like displaying "Hello World".

---

## 🧠 General Registers

| Register | Size         | Description                                                                                                 |
| -------- | ------------ | ----------------------------------------------------------------------------------------------------------- |
| `AH`     | 8-bit (high) | The high byte of the 16-bit AX register. Often used to store function numbers for BIOS or DOS interrupts.   |
| `AL`     | 8-bit (low)  | The low byte of the AX register. Commonly used to hold data, such as characters to print.                   |
| `AX`     | 16-bit       | A general-purpose register made up of AH and AL. Often used as an accumulator.                              |
| `BH`     | 8-bit (high) | The high byte of the 16-bit BX register.                                                                    |
| `BL`     | 8-bit (low)  | The low byte of the BX register.                                                                            |
| `BX`     | 16-bit       | Base register, often used for addressing memory.                                                            |
| `CH`     | 8-bit (high) | The high byte of the 16-bit CX register.                                                                    |
| `CL`     | 8-bit (low)  | The low byte of the CX register.                                                                            |
| `CX`     | 16-bit       | Count register, commonly used for loop counters and string operations.                                      |
| `DH`     | 8-bit (high) | The high byte of the 16-bit DX register.                                                                    |
| `DL`     | 8-bit (low)  | The low byte of the DX register.                                                                            |
| `DX`     | 16-bit       | Data register, used in I/O operations and multiplication/division.                                          |
| `SI`     | 16-bit       | Source Index register. Points to memory for string operations like `LODSB`.                                 |
| `DI`     | 16-bit       | Destination Index register. Used in string operations like `STOSB`.                                         |
| `SP`     | 16-bit       | Stack Pointer. Points to the top of the stack. Works with `SS`.                                             |
| `BP`     | 16-bit       | Base Pointer. Used to access function parameters and local variables on the stack.                          |
| `IP`     | 16-bit       | Instruction Pointer. Points to the offset of the next instruction within the code segment. Works with `CS`. |

---

## 🧱 Segment Registers

| Segment Register | Size   | Description                                                                                                                                                         | Paired With            |
| ---------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------- |
| `CS`             | 16-bit | Code Segment. Specifies the memory segment where the currently executing instructions (code) are located. Determines where the CPU fetches instructions to execute. | `IP`                   |
| `DS`             | 16-bit | Data Segment. Default segment for most data (variables, arrays). Most data reads and writes use this segment unless overridden.                                     | `SI`, `BX`, `AX`, etc. |
| `SS`             | 16-bit | Stack Segment. Specifies the memory segment used for the stack (function calls, returns, temporary storage, PUSH/POP operations).                                   | `SP`, `BP`             |
| `ES`             | 16-bit | Extra Segment. Used as an additional data segment, often as the destination in string and memory operations (e.g., moving data from `DS:SI` to `ES:DI`).            | `DI`                   |

---

## 🧮 How Physical Memory Addresses Are Calculated

In the 8086 processor, physical memory addresses are calculated using **segment:offset addressing**. Since all registers are 16-bit (maximum value = 0xFFFF), but the processor can access up to **1MB** of memory, it uses the following formula:

`Physical Address = (Segment × 16) + Offset`

This effectively allows 20-bit physical addressing using two 16-bit values.

---

## 🛠️ Instructions

| Instruction | Description                                                                                  |
| ----------- | -------------------------------------------------------------------------------------------- |
| `mov`       | Transfers data between registers or between a register and memory.                           |
| `jmp`       | Unconditional jump to a specified label or memory address.                                   |
| `je`        | Jump if equal (Zero flag is set); typically used after `cmp`.                                |
| `int`       | Calls an interrupt. `int 10h` is commonly used for BIOS video services.                      |
| `lodsb`     | Loads a byte from the address pointed to by `SI` into `AL`, and increments `SI`.             |
| `call`      | Calls a procedure (subroutine); pushes return address to stack.                              |
| `ret`       | Returns from a procedure; pops return address from stack.                                    |
| `cmp`       | Compares two values by subtracting one from the other, setting flags but not storing result. |

---

## 🔍 Concepts to Understand

### ✅ Memory Layout

- `.COM` files are loaded at `0x100` in memory.
- Execution starts from `org 100h`, ensuring correct offsets for jumps and data.

### ✅ Data Declarations

- `db 'Hello World!', 0` stores the string in memory.
- `0` at the end is a null terminator used to indicate the end of the string.

### ✅ Execution Flow

- The CPU reads and executes instructions linearly.
- You must use `jmp` to skip over data sections like the string, otherwise it will interpret data as code and crash.

### ✅ BIOS Interrupts

- `int 10h` with `AH = 0Eh` prints a character from `AL` to the screen.
- BIOS provides low-level hardware access while in real mode.

### ✅ Printing a String

- Use `SI` to point to the beginning of the string.
- Use `lodsb` to load each character and automatically advance the pointer.
- Compare `AL` with `0` to check for the end of the string.
- Use a loop to print each character with `int 10h`.

---

## 🧪 Example Code Recap

```asm
org 100h
jmp main

message: db 'Hello World!', 0

print:
  mov ah, 0Eh
.loop:
  lodsb
  cmp al, 0
  je .done
  int 10h
  jmp .loop
.done:
  ret

main:
  mov si, message
  call print
  ret
```
