# MIPS Instruction Encoding

> 一共有三種類型
> 
> R, I , J
> 
> R 的通常會用在 
> 
> + `xxx reg1, reg2, reg3` 
> 
> + sll, srl, sllv, srlv `sll reg1, reg2, num`, `sllv reg1, reg2, reg3`
> 
> I 的通常會用在有立即值的
> 
> + `xxx reg1, reg2, num`
> 
> + `lw reg1, num(reg2)`

## Format R

### i. add, sub, and, or, xor, nor

`add $t0, $s1, $s2`

| opcode | 第2個   | 第3個   | <mark>第1個</mark> | shift amount | function code |
|:-------:|:-----:|:-----:|:----------------:|:-----:|:------:|
| special | \$s1 | \$s2          | \$t0 | 0   |add|
| 000000  | 10001 | 10010 | 01000            | 00000 | 100000 |

### ii-i. Shift type 1 (有立即值), sll, srl

+ sll: shift left logical
+ srl: shift right logical

`srl $t0, $s1, 2`

| opcode | 空     | 第2個   | <mark>第1個</mark> | num   | function code |
|:-------:|:-----:|:-----:|:----------------:|:-----:|:------:|
| special | 0     | \$s1            | \$t0 | 2   |srl|
| 000000  | 00000 | 10001 | 01000            | 00010 | 000010 |

### ii-ii Shift type 2 (沒有立即值) sllv, srlv

+ sllv: shift left logical variable
+ slrv: shift right logical variable

`sllv $t0, $s1, $s0`

| 只有兩種    | 第3個   | 第2個   | <mark>**第1個**</mark> | 空     | opcode |
|:-------:|:-----:|:-----:|:--------------------:|:-----:|:------:|
| special | \$s0 | \$s1             | \$t0 | 0  |sllv|
| 000000  | 10000 | 10001 | 01000                | 00000 | 000110 |

## Format I

### i. addi, andi, ori, xori

`addi $t0, $s0, 61`

| opcode | 第 2 個 | 第 1 個 | 立即值 |
|:------:|:-----:|:-----:|:---:|
| 001000 | 10000 | 01000 | 61  |

### ii. lw

以 `$s3` 的值加 40 後得到的位址為基準，將含基準的連續4個bytes 丟進 `$t8`

`lw $t8, 40($s3)`

| opcode | 第 2 個 base |   第 1 個   | 位移量立即值 |
| :----: | :----------: | :---------: | :----------: |
| 100011 | 10011 `$s3`  | 11000 `$t8` |      40      |

`sw $s4, -8($t9)`

| opcode | 第 2 個 base |   第 1 個   | 位移量立即值 |
| :----: | :----------: | :---------: | :----------: |
| 101011 | 11001 `$t9`  | 10100 `$s4` |      -8      |

### iii-i. beq, bne

`beq $s1, $s2, L`

| opcode | 第 1 個 | 第 2 個 | 相對位址  |
|:------:|:-----:|:-----:|:-----:|
| 000100 | 10001 | 10010 | label |

#### 計算相對地址

**公式**

$$
Relative\ branch\ distance = (target\ addr - beq\ addr-4)/4
$$

## Format J

Target address calculation on jump

`J SKIP` 這行若在`18000004` 

而 SKIP 在 `1800001C`

<mark>將左邊 4 個 bits 去掉，右邊 2 個 bits 去掉</mark>

~~0001~~ 1000 0000 0000 0000 0000 0001 11~~00~~

| opcode  | address                          |
|:-------:|:--------------------------------:|
| 0000 10 | 10 0000 0000 0000 0000 0000 0111 |

+ procedure call: jump and link `jal procedureLabel`
  + address of following insturction put in `$ra` (將下一行指令位置放入 `$ra`)
  + jump to target address.
+ procedure return: `jr $ra`
  + Copies `$ra`to program counter
  + can also be used for computed jumps, e.g., for case/switch statements.
