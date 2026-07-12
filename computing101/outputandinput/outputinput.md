# System Calls

It's an instruction that makes a call into the Operating System. syscall triggers the system call specified by the value in rax. arguments in rdi, rsi, rdx, r10, r8 and r9. Return value in rax.

## Stdin & Stdout

### Reading 100 bytes from stdin to the stack:

```asm
n = read(0, buf, 100);

mov rdi, 0 # the stdin file descriptor
mov rsi, rsp # read the data onto the stack
mov rdx, 100 # the number of bytes to read
mov rax, 0 # system call number of read()
syscall # do the system call
```

read returns the number of bytes read via rax, so we can write them out:

```asm
write(1, buf, n);

mov rdi, 1 # the stdout file descriptor
mov rsi, rsp # write the data from the stack
mov rdx, rax # the number of bytes to write (same as what we read in)
mov rax, 1 # system call number of write()
syscall # do the system call
```

## Strings arguments

Some system calls take string arguments (fire path, e.g). A string is a group of contiguous bytes in memory followed by a **0 byte** (`\0`).

```asm
mov BYTE PTR [rsp+0], '/' # write the ASCII value of / onto the stack
mov BYTE PTR [rsp+1], 'f'
mov BYTE PTR [rsp+2], 'l'
mov BYTE PTR [rsp+3], 'a'
mov BYTE PTR [rsp+4], 'g'
mov BYTE PTR [rsp+5], 0 # write the 0 byte that will terminate our string
```

To open the /flag file:
```asm
mov rdi, rsp # read the data onto the stack
mov rsi, 0 # open the file read only (more on this later)
mov rax, 2 # system call number of open()
syscall # do the system call
```

## Using RIP

```asm
.intel_syntax noprefix
.global _start
_start:
lea rdi, [rip+path]
syscall
path:
.asciz "/flag"
```
3) lea vs mov

This is the big idea:

    lea gives you an address
    mov usually gives you the data at that address (if memory is involved)

Example memory

Suppose:

    the label path is at address 0x40103a
    memory at that address contains "/flag\0"

A) lea rdi, [rip + path]

This means:

    “Compute the address of path, and put that address into rdi.”

So after this:

rdi = 0x40103a

That is what open wants: a pointer to the filename.
B) mov rdi, [rip + path]

This means:

    “Go to that address in memory, read bytes from there, and copy the value into rdi.”

So instead of getting the address 0x40103a, you get the contents starting there.

Those bytes are:

2f 66 6c 61 67 00 ...

So rdi becomes some integer made from those bytes, roughly:

rdi = 0x0067616c662f   ; conceptually from "/flag\0"

That is not a valid pointer to the string.
It is just the bytes of the string packed into a register.

So:

    lea → pointer to /flag
    mov [memory] → bytes of /flag
