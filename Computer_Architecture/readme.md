#  计算机体系结构

## 指令集架构 Instruction Set Architecture

四种主要ISA:Stack,Accumulator,Register-Memory,Load-Store.

现代的处理器多为Reg-Mem或者Load-Store的

- Stack

似乎已经没有流行的硬件.这种结构对寄存器没有特别的要求.在各种高级语言虚拟机(HLL VM)中,非常地流行.

- Load-Store

可以肯定它是一个RISC处理器,那么它必有pipeline.如经典的MIPS的5段流水线和ARM的3段流水线.

### 指令集的实现

## 来源

整理自:

[面试宝典3：计算机体系结构](https://csbabel.wordpress.com/2010/10/31/interviewbible-3-computer-architecture/amp/?__twitter_impression=true)