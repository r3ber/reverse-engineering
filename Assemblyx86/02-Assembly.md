# Assembly

As i had written in the previous note (01-ComputerArch.md), whatever is the programming language we use, eventually it will go to a binary program and it will be executed by the CPU instruction by instruction.

## Table of Contents

- [Assembly](#assembly)
  - [Table of Contents](#table-of-contents)
    - [Binary](#binary)
    - [History](#history)
    - [Operands](#operands)
    - [Operations](#operations)
    - [Assembly Dialects](#assembly-dialects)
      - [Dialects of Assembly Dialects](#dialects-of-assembly-dialects)


### Binary

We as humans have a hard time with binary code so we created a text representation equivalent to binary called **Assembly**.

### History

Invented by Kathleen Booth, it's named Assembly because it's assembled (not compiled) into binary code.

### Operands

The Operands deal with Data. Most time, CPU it's concerned with three types of data:

- data we give directly as part of the instruction
- data that is close at hand, it receiver data and put's it in a register
- data in storage (not a register)

### Operations

- add some data
- **sub**tract some data
- **mul**iply some data
- **div**ide some data
- **mov**e some data into or out of storage
- **c**o**mp**are two pieces of data with each other

### Assembly Dialects

Assembly depends on CPU architecture. Every arch has its own variant:

- x86 Assembly
- arm Assembly
- MIPS Assembly

#### Dialects of Assembly Dialects

There are two versions of x86, one from intel (they made the arch so it's obviosly better) and another one from AT&T (terrible)
