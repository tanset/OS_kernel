# 📘 8086 Assembly Language Quick Reference

This document summarizes key registers, instructions, and essential concepts used in 8086 assembly programming, with a focus on real-mode applications like displaying "Hello World".

---

## 🧠 Registers

| Register | Size         | Description                                                                                                                                     |
| -------- | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| `AH`     | 8-bit (high) | The high byte of the 16-bit AX register. Often used to store function numbers for BIOS or DOS interrupts.                                       |
| `AL`     | 8-bit (low)  | The low byte of the AX register. Commonly used to hold data, such as characters to print.                                                       |
| `AX`     | 16-bit       | A general-purpose register made up of AH and AL. Often used as an accumulator.                                                                  |
| `SI`     | 16-bit       | Source Index register. Commonly used to point to a memory location (e.g., for string or array operations). Used with instructions like `lodsb`. |

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
