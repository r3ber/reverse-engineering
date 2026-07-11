# The Stack

## Description

So far, we've been loading data from memory at addresses that we gave you: either hardcoded (like 133700) or stored in a register (like rax). But there's one region of memory that your program already has access to without any setup from us: the stack.

The stack is a region of allocated memory used as scratch space for your program, and the register rsp (the Stack Pointer) points to the top of it. We'll explore the stack further later, but for now, the relevant detail is this: when a program starts, rsp points to data that represents the number of command-line arguments passed to the program (including the program name itself).

So if you run:

```bash
hacker@dojo:~$ /tmp/your-program hello world
```

Then the situation looks like this (the actual addresses are an example):

```bash
    Address    │ Contents
  +───────────────────────+
  │ ...        │ ...      │
  +───────────────────────+
┌▸│ 1337000    │ 3        │  ◀── the argument count
| +───────────────────────+
| | 1337008    | ???      |
| +───────────────────────+
| | 1337016    | ???      |
│ +───────────────────────+
│
└────────────────────────────┐
                             │
   Register │ Contents       │
  +────────────────────────+ │
  │ rsp     │ 1337000      │─┘
  +────────────────────────+
```

rsp points to the stack, and the value there is 3: one for the program name, one for hello, and one for world. The stack also has other data, as shown, but we won't worry about that for now!

In this challenge, read the argument count from [rsp] and use it as the exit code of your program. We'll run your program a few times with different arguments to make sure you're reading it correctly!

# Notes

- Important to remember: RSP (Stack Pointer) points to the top of the stack. When the program starts, the value at [RSP] is the number of command-line arguments passed to the program, including the program name itself.
- So to read the number of arguments passed to the program we can just dereference the rsp register and use that value to do a number of things, but in this case, we need to pass it to rdi in order to use it as the exit code.

```asm
.global _start
_start:
  mov rdi, [rsp]
  mov rax, 60 ; exit code for syscall
  syscall
```

- We can use offsets to access the stack, for example, if we want to access the address that contains the first argument (not the program name), we can use [rsp + 16]. For the second argument (hello) we can use [rsp + 24], and so on.
- If we want to actually access the value of the first argument, for example, we need to dereference the pointer at [rsp + 16], this gives us the address of the first argument, and then dereference that pointer to get to the actual value.