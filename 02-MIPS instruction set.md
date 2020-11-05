# MIPS Instruction Set

## Overview

MIPS 與先前學的 x86 有一些不同，MIPS 是採用 Big endian，另外，MIPS 的 word 是指 4 個 bytes，與 MASM, NASM 的習慣不同

MIPS 一共有 32 個暫存器，如同 x86 一樣一個暫存器中可以存取 32 個 bits

另外比較特殊的暫存器如：

1. \$zero 永遠存著 32 bits 的 0
2. \$ra = return address

## Operand Alignment

1. byte

2. half word (2 bytes)
   
   2k, 2k+1

3. word (4 bytes)
   
   4k, 4k+1, 4k+2, 4k+3

4. double word (8 bytes)
   
   8k, 8k+1, 8k+2, ..., 8k+7

## Big Endian vs. Little Endian

0x1234ABCD

**Little endian(intel)**

| i   | CD  |
|:---:|:---:|
| i+1 | AB  |
| i+2 | 32  |
| i+3 | 12  |

**Big endian (Motorala, MIPS)**

| i   | 12  |
|:---:|:---:|
| i+1 | 34  |
| i+2 | AB  |
| i+3 | CD  |

## Memory operand example

### Example1

**C code**

```c
g = h + A[8]
// g : $s1
// h : $s2
// A : $s3
```

**MIPS**

```
lw    $t0, 32($3)
add   $s1, $s2, $t0
```

### Example2

**C code**

```c
A[12] = h + A[8]
// h : $s2
// A : $s3
```

**MIPS**

```
lw    $t0, 32($s3)
add   $t0, $s2, $t0
sw    $t0, 48($s3)
```

> 注意 lw (load word) 和 sw (store word) 不太一樣

## MIPS Instruction Formats

| type | 26-31  | 21 - 25             | 16 - 20      | 11 - 15                             | 6 - 10       | 0 -5              |
|:----:|:------:|:-------------------:|:------------:|:-----------------------------------:|:------------:|:-----------------:|
| R    | Opcode | src reg1            | src reg2     | dest reg                            | shift amount | op code extension |
| I    | Opcode | src or base         | dest or data | immediate operand or address offset | 同左           | 同左                |
| J    | Opcode | memory word address | 同左           | 同左                                  | 同左           | 同左                |
