# Computer Architecture

## Table of Contents
- [Computer Architecture](#computer-architecture)
  - [Table of Contents](#table-of-contents)
  - [Fundamentals](#fundamentals)
    - [Logic Gates](#logic-gates)
      - [AND GATE](#and-gate)
      - [OR GATE](#or-gate)
      - [NOT GATE](#not-gate)
      - [NAND GATE](#nand-gate)
      - [NOR GATE](#nor-gate)
      - [XOR GATE](#xor-gate)
      - [XNOR GATE](#xnor-gate)
  - [High Level - Computer Architecture](#high-level---computer-architecture)
    - [CPU](#cpu)
      - [ALU (Arithmetic Logic Unit)](#alu-arithmetic-logic-unit)
      - [CU (Control Unit)](#cu-control-unit)
      - [Registers](#registers)
      - [L1 and L2 Cache](#l1-and-l2-cache)
        - [L1 (Level 1)](#l1-level-1)
        - [L2 (Level 2)](#l2-level-2)
    - [Memory](#memory)
    - [Disk](#disk)
    - [Network and others](#network-and-others)
      - [Network](#network)
      - [Buses](#buses)


## Fundamentals

Source code (e.g., Python, JavaScript, Java, C, C++, Rust) is first compiled or interpreted into an intermediate language such as bytecode(Interpreter or JIT) or (Compiler) and then binary-encoded instructions that run on the CPU.

### Logic Gates

At the center of the CPU there are many many many logic gates. The most important are listed below. One CPU is made with a mix of these gates:

#### AND GATE

| A | B | Out |
|---|---|:---:|
| 0 | 0 |  0  |
| 0 | 1 |  0  |
| 1 | 0 |  0  |
| 1 | 1 |  1  |

#### OR GATE

| A | B | Out |
|---|---|:---:|
| 0 | 0 |  0  |
| 0 | 1 |  1  |
| 1 | 0 |  1  |
| 1 | 1 |  1  |

#### NOT GATE

| A | Out |
|---|:---:|
| 0 |  1  |
| 1 |  0  |

#### NAND GATE

| A | B | Out |
|---|---|:---:|
| 0 | 0 |  1  |
| 0 | 1 |  1  |
| 1 | 0 |  1  |
| 1 | 1 |  0  |

#### NOR GATE

| A | B | Out |
|---|---|:---:|
| 0 | 0 |  1  |
| 0 | 1 |  0  |
| 1 | 0 |  0  |
| 1 | 1 |  0  |

#### XOR GATE

| A | B | Out |
|---|---|:---:|
| 0 | 0 |  0  |
| 0 | 1 |  1  |
| 1 | 0 |  1  |
| 1 | 1 |  0  |

#### XNOR GATE

| A | B | Out |
|---|---|:---:|
| 0 | 0 |  1  |
| 0 | 1 |  0  |
| 1 | 0 |  0  |
| 1 | 1 |  1  |

## High Level - Computer Architecture

We have Memory, Disk, Network and other things. We then have some sort of bridge that is used for these components to communicate with CPU.

### CPU

It's the Central Processing Unit of the computer (aka the "brain"). It's responsible for executing instructions, in a cycle known as **Fetch-Decode-Execute**. It processes data based on the logic gates mentioned above. As components we have:

#### ALU (Arithmetic Logic Unit)

The calculator of the CPU. It performs all arithmetic (addition, subtraction) and logical operations (AND, OR, XOR).

#### CU (Control Unit)

It directs the flow of data between the CPU and the other components, telling ALU and memory how to respond to instructions.

#### Registers

Extremely fast, small storage inside CPU. They hold the immediate data being processed.

#### L1 and L2 Cache

High-speed memory located physically inside the CPU chip.

##### L1 (Level 1)

The fastest and smallest, usually dedicated to each core.

##### L2 (Level 2)

Larger and slower than L1 but still much faster than RAM. It's used to prevent CPU from waiting


### Memory

We have **RAM** (**R**andom **Access** **M**emory), it's where computer store **data** and **instructions** that the CPU is currently processing. It's **volatile**, which means that data is lost when power is turned off. It's **faster than a Disk** but **slower than a CPU register** or **Cache**


### Disk

Unlike memory, it is non-volatile, long-term storage for the Operating System. it keeps data even without power.

It's **slower to use the disk** to go grab some stuff **instead of just using registers**.

When RAM is full, the OS uses part of the disk to use as extra memory in what's called **Swapping**, with the downside of slowing down the system.

### Network and others

Here are the I/O (Input/Output) systems that allow the computer to interact with the outside world.

#### Network

Is managed by the NIC (Network Interface Card). Data arrives in packets and it goes to memory via the System Bus.

#### Buses

These are the **bridge** that I meantioned earlier are **buses**. As examples we have, **PCIe** for GPU/NVMe, **SATA** for old disks and **USB** for peripherals.
