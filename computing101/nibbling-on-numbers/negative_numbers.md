# Two's Complement

Two's complement works the same at any width --- these are 1 byte (8 bits) each.
The top bit is still the sign. If it's 0 the value is positive and its signed and
unsigned readings are identical; if it's 1 it's negative, and the signed value is
the unsigned value minus 2**8 (= 256).

## (16-bit) negative numbers

Two's complement works the same at any width --- these are 2 bytes (16 bits) each.
The top bit is still the sign. If it's 0 the value is positive and its signed and
unsigned readings are identical; if it's 1 it's negative, and the signed value is
the unsigned value minus 2**16 (= 65536).

## (32-bit) negative numbers

Two's complement works the same at any width --- these are 4 bytes (32 bits) each.
The top bit is still the sign. If it's 0 the value is positive and its signed and
unsigned readings are identical; if it's 1 it's negative, and the signed value is
the unsigned value minus 2**32 (= 4294967296).

# Encoding negative

Now the other direction: I give you a number, and you encode it in 8-bit
two's complement, written in binary.
A zero-or-positive number is just its plain binary. For a negative number, its
8-bit pattern is the same bits as the unsigned value (the number + 256), so its
top bit ends up set.
