# Disassembling Programs

The process of converting the binary machine code in an executable back into human-readable assembly instructions.

## objdump

```bash
objdump -d -M intel /tmp/your-program
```
_Note: By default, objdump uses the wrong assembly syntax, which is why we pass the -M intel option._

## Tracing Syscalls

Given a program to run, strace will use functionality of the Linux operating system to introspect and record every system call that the program invokes, and its result.

```bash
strace /tmp/your-program
```

### Syscall number 37

For example, the alarm system call (syscall number 37!) will set a timer in the operating system, and when that many seconds pass, Linux will terminate the program. The point of alarm is to, e.g., kill the program when it's frozen

## GDB

GDB stands for the GNU Debugger, and it is typically used to hunt down and understand bugs. More specifically, a debugger is a tool that enables the close monitoring and introspection of another process.
```bash
gdb /path/to/binary/file
```

### Starting Programs in GDB

Debuggers, including gdb, observe the debugged program as it runs to expose information about its runtime behavior.

You start a program with the starti command:
```bash
(gdb) starti
```

`starti` starts the program at the very first instruction. Once the program is running, you can use other gdb commands to inspect its actual runtime state. We'll start with the code that's running, which you can disassemble using the disassemble command!

### Stepping through instruction

However, even though you can't read the value from the code, you can still execute the code! When the CPU executes mov rdi, CENSORED, it loads the actual secret value into the rdi register.

To execute a single instruction in GDB, use the stepi command (step one instruction, also abbreviated si):

(gdb) stepi

Once you step past the mov instruction, we'll read the rdi register for you and show the secret value.

### Reading Register Values
```bash
starti
stepi
(gdb) print $rdi
```

### Setting Register Values

GDB can also change a register while the program is stopped. The set command assigns a new value to a register:
`(gdb) set $rax = 42`

As with print, prefix the register name with `$`.

### Popping Stack Values

In previous levels, the secret was hidden in the program's code (a hardcoded mov instruction). This time, the secret comes from the program's runtime state: it's the argument count (argc), which lives on the stack.

The program pops this value off the stack with pop rdi, but then immediately overwrites rdi with 0. So we need to step into the instruction and then print the rdi register value.

### Examining Memory

In this level, there is no pop rdi but the secret is still in argc. In order to do that we can examine the memory in the stack with the command x (e**x**amine):

```bash
x $rsp
```

### Examining Stack Pointers

the stack holds more than just argc!

Right after the argument count, the stack stores pointers to each program argument. These are addresses stored in memory: $rsp+16 doesn't contain the argument text directly --- it contains the address where that text lives.

For example, if your program is run as /challenge/debug-me Hi:

     Address    │ Contents
   +────────────────────────────+
   │  rsp + 0   │ 2             │◀── argc
   +────────────────────────────+
   │  rsp + 8   │ 0x1234000     │──────┐
   +────────────────────────────+      │
   │  rsp + 16  │ 0x1234560     │────┐ │
   +────────────────────────────+    │ │
                                     │ │
                                     │ │
     Address    │ Contents           │ │
   +──────────────────────────────+  │ │
   │ 0x1234000  │ "/challenge/..."│◀─│─┘ the program name
   +──────────────────────────────+  │
   │ ...        │ ...                │
   +──────────────────────────────+  │
   │ 0x1234560  │ "Hi"            │◀─┘   the first argument
   +──────────────────────────────+

To get the actual argument data, you need two dereferences: one to get the pointer from the stack, and one to follow it to the string.

In this level, THE FLAG ITSELF is passed as the first argument! The program doesn't use it --- it just exits --- but the flag is right there in memory.

To find it, you'll need two x commands, with two different display modes:

First: You'll need the pointer the first argument. You've done this before, but now you're doing it in gdb.

x/a $rsp+16

/a tells x to display the value as a memory address. You'll see a very large hexadecimal number, something like 0x7ffc001c4750.

Second: Read the text of the first argument at that address:

x/s 0x7ffc001c4750

/s tells x to display the value as a string. Replace the address with whatever you got from step 1. This will show you the flag!

### Cooperative Debugging

So far, the debugging you've done has been preemptive: you (the debugger) started the program with stepi, which immediately forces it to stop and let you debug it, without the program necessarily being aware of it. In this challenge, we'll learn another model for this, where the program decides when the debugger stop happens. We'll call this cooperative debugging.

On our now-familiar x86 architecture, the program can signal a desire to be debugged by using the int3 instruction. If a debugger is attached when int3 is executed, it stops the program. This is called a program breakpoint.

Later, we'll learn how to set breakpoints from the debugger itself, going back to the preemptive model. But in this challenge, the checker will run your program under gdb and expect your program to trigger its own breakpoint. To do this, rather than using starti to start your program and immediately stop it, we'll use gdb's run command, which will simply run it until a breakpoint is hit!

### Running with arguments

what if the program needs command-line arguments to work?

Outside gdb, you've been passing arguments by just typing them after the program name:

`hacker@dojo:~$ /challenge/debug-me hello`

Inside gdb, the analog is to pass them to run:

`(gdb) run hello`

Whatever you put after run becomes the inferior's argv[1], argv[2], and so on --- exactly as if you'd typed those arguments on the shell command line. GDB also accepts the short form r:

`(gdb) r hello`

(Anywhere you see run in gdb's docs, r works too.)

### Redirecting Input in GDB

In the previous level, you passed command-line arguments through gdb's run. Programs can also read from stdin, and gdb lets you redirect stdin when you run the inferior. The syntax is the same redirection you've seen in the shell, but it goes after run inside gdb:

```bash
(gdb) run < /path/to/input
```
