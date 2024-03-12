Access Mars

cd /course/cs365-/MIPS-simulator/
ls

https://ood.discovery.neu.edu:5554/auth/ldap/login?back=&state=phcr5zzs5lix3jm7hhaejpszf
ion really know what the fuck to do from here...

32 variables

load and store variables from RAM
home for a variable is RAM
can temporarily use a hardware variable and then load it into RAM

lw, sw <-- load word, store word

sit on top of CPU![[Sit on CPu.png]]
lw \$r3, 4(\$sp)
		^ basically a pointer / address
C arrays:
sp\[4]
points to some place in memory
or:
\*(sp + 4)

\$r3, 4(\$sp)
means loading in 4 byte offset in \$sp to \$r3

register is a 4-byte word
\(32 bits)

32 bit CPU

8 bytes is 2 arrays. if you're working with int arrays, you have to divide by 4
add \$r7, \$r5, \$r4 \#in C: r7 = r5 + r4
sub \$r7, \$r5, \$r4 \#in C: r7 = r5 - r4
addi \$r7, \$r5, 1 \#in C: r7 = r5 + 1
addu \$r7, \$r5, \$r4 \#thinking about it like an unsigned int (only positive).
addiu \$r7, \$r5, 1 \#unsigned with a small constant
and, or, neg \# &&, ||, !

#### if

```c
if (x == y) {z = 5};
```
```assembly
mylabel:
  if (x != y) {goto skip;}
  z = 5
  skip:
```
```
beq $r5, $r6, skip
addi $r8, $zero, 5
skip:
```
#### then

#### else

#### while
```asm
loop:
if (x > 5) {goto skip;}
  ++x;
  goto loop
skip:
```
#### for

#### function calls


## Assembler Directives

```
.data
	x: .word  # basically int x;
	y: .word
	z: .word

	x: .word 12 # basically int x = 12;
```
3 things we can do with a symbol is:
1. Declare type
2. Allocate storage
3. (optional) Initialize the symbol

```
.text
	lw \$r5, x
	addi ~~~~
	~~~~~~~~~ 

Function:

jal foo # jumps to foo function

foo: # function foo
~~~~
~~~~
~~~~
~~~~
jr $ra # returns from foo function
```