# Instruction Set

# Lovelace

## Registers
- `r0`, `r1`, `r2`, `r3` 8-bit general registers
- `pc`, 16-bit program counter
- `acc` 8-bit accumulator
- `sp` 8-bit stack pointer
- `flags` 8-bit flags register
 - `sign` `x0000000`
   - Sign Flag
   - If an instruction's result is a value this flag is set to bit 7 of that value.
 - `carry` `0x000000`
   - Carry Flag
   - After an addition or subtraction instruction, set if the 9th bit would have been 1.
 - `interrupt` `00x00000`
   - Interrupt Disable Flag
   - Is only manually set. Determines whether or not interrupts happen.
 - `overflow` `000x0000`
   - Overflow Flag
   - Indicates that signed integer overflow occurred.
 - `greater` `000000x0`
   - Greater than flag
   - Set if the last `cmp` instruction found a positive difference, otherwise it is cleared.
 - `zero` `0000000x`
   - Zero flag
   - If an instruction's result is a value this flag is set if that value is 0, otherwise it is cleared.
   - Set if the last `cmp` instruction found no difference, otherwise it is cleared.


A "value register" is either a general register or the accumulator.

## Instructions

Data movement operations are in the form `{source}, {destination}`.

### No operation (1 opcode)

- `nop` (1 byte, 1 cycle)

### Data movement (42 opcodes + 8 zero-page)

- `store {general register}, {address}` (3 bytes, 4 cycles, 4 opcodes)
- `load {address}, {general register}` (3 bytes, 4 cycles, 4 opcodes)
- `load {literal}, {general register}` (2 bytes, 2 cycles, 4 opcodes)
- `trans {8-bit register}, {value register}` (1 byte, 1 cycle, 30 opcodes)

### Accumulator operations (54 opcodes)

- `add {general register}` (1 byte, 1 cycle, 4 opcodes)
- `addc {general register}` (1 byte, 1 cycle, 4 opcodes)
- `sub {general register}` (1 byte, 1 cycle, 4 opcodes)
- `subc {general register}` (1 byte, 1 cycle, 4 opcodes)
- `shr {literal}` (2 bytes, 2 cycles, 1 opcode)
- `shr {value register}` (1 byte, 1 cycle, 5 opcodes)
- `shl {literal}` (2 bytes, 2 cycles, 1 opcode)
- `shl {value register}` (1 byte, 1 cycle, 5 opcodes)
- `and {general register}` (1 byte, 1 cycle, 4 opcodes)
- `and {literal}` (2 bytes, 2 cycles, 1 opcode)
- `or {general register}` (1 byte, 1 cycle, 4 opcodes)
- `or {literal}` (2 bytes, 2 cycles, 1 opcode)
- `xor {general register}` (1 byte, 1 cycle, 4 opcodes)
- `xor {literal}` (2 bytes, 2 cycles, 1 opcode)
- `not` (1 byte, 1 cycle, 1 opcode)


- `inc {value register}` (1 byte, 1 cycle, 5 opcodes)
- `dec {value register}` (1 byte, 1 cycle, 5 opcodes)

### Subroutine instructions (3 opcodes)

- `jsr {address}` (3 bytes, 5 cycles, pushes 2)
- `ret` (1 byte, 3 cycles, pops 2)
- `reti` (1 byte, 4 cycles, pops 3)

### Stack instructions (12 opcodes)

- `push {value register}` (1 byte, 2 cycles, 5 opcodes)
- `push {literal}` (2 bytes, 3 cycles, 1 opcode)
- `pop {value register}` (1 byte, 2 cycles, 5 opcodes)
- `drop` (1 byte, 1 cycle, 1 opcode)

### Jump instructions (38 opcodes)

- `cmp {value register}, {value register}` (1 byte, 1 cycle, 20 opcodes)
- `cmp {value register}, {literal}` (2 bytes, 2 cycles, 5 opcodes)
- `bra {flag}, {address}` (3 bytes, 3 cycles, 6 opcodes)
- `brn {flag}, {address}` (3 bytes, 3 cycles, 6 opcodes)
- `jmp {address}` (3 bytes, 3 cycles, 1 opcode)

### Flag instructions (12 opcodes)

- `set {flag}` (1 byte, 1 cycle, 6 opcodes)
- `clear {flag}` (1 byte, 1 cycle, 6 opcodes)

## Pins

- Clock pin
- Reset pin
- 16 bit address bus
- 8 bit data bus
- read/write pin
