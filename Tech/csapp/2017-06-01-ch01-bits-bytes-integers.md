# CSAPP: Bits, Bytes, and Integers

## Representing information as bits

### Encoding Byte values

> Byte = 8 bits

- Binary: 00000000 to 11111111
- Decimal: 0 to 255
- Hexadecimal: 00 to FF

### Byte-Oriented Memory Organization

- Programs refer to **Virtual Address**
- Compiler + Run-Time system control allocation

### Machine Words

machine has "word size"

- indicating the nominal size of integer and pointer data.
- the most important system parameter determined by the word size is _the maximum size of the virtual address space_.
- use 32 bits (4 bytes) words, limits addresses to 4GB.

### Word-Oriented Memory Organization

Addresses specify byte locations

- Address of first byte in word
- Address of successive words differ by 4 (32-bit) or 8 (64-bit)