## What is the performance we care about?

### 1. Response time

> How long it takes to do a task

### 2. Throughput

> Total work done per unit time

## CPU TIME

CPU TIME =  CPU Clock Cycles × Clock Cycle Time = CPU Clock Cycles / Clock Rate

### performance improved by

+ Reducing number of clock by cycles (靠演算法、編譯器)

+ Increasing clock rate (有物理上的限制)

+ Hardware designer must often trade off clock rate against cycle count

## Instruction count and CPI

**CPI: Cycles per Instruction**

+ Clock Cycles = Instruction Count × Cycles per Instruction

+ CPU Time = Instruction Count × CPI × Clock Cycle Time

> **Instruction count for a program:**
> Determined by program. ISA (Instruction Set Architecture 指令集架構)

> **Average cycles per instruction:**
> 
> Determined by CPU hardware
> 
> If different instructions have different CPI

## Performance summary

**Depends on**

1. Algorithms

2. Programming language

3. Compiler

4. ISA (Instruction set architecture)
