### ASM

jal: jump and link
1. sets $ra to $pc + 4 (next instruction)
	1. "Link"
2. Sets $pc to foo
	1. "Jump"
---

Labels:

Functions
```
foo: ~~~
     ~~~
     ~~~
     jr $ra #return
```

Flow control: (if, while, for)
```
while (x > 0) {}
loop: ble $t5, $zero, skip # branch if less than or equal zero, goto skip. we assume $t5 to be x
	  ...
      b loop # branch straight to loop
skip:

bgt # branch greater than 
bge # branch greater than or equal to
blt # branch less than
ble # branch less than or equal to
bne # branch not equal to
```

#### [GreenCard](https://inst.eecs.berkeley.edu/~cs61c/resources/MIPS_Green_Sheet.pdf)
![[Green Card.png]]

Functions: 
	jal (jump and link)
	jr $ra (return from return address)
Branch Instructions: 
	beq $t5, $t6, mynext
	if (t5 == t6) {go to mynext;}
Assignment Statements:

	add $t7, $t4, $t5
	t7 = t4 + t5;
	
	addi $t7, $t4, 42
	t7 = t4 + 42
	
	li $t7, 42
	load immediate 
	immediate usually means small constant
	addi $t7, $zero, 42
	
	sub,
	and,
	andi...
Load-store
	CPU chip that has 32 registers, ALU, etc
	lw \$t8, 8($sp) <--> same thing as sp\[8] <--> 8 byte offset | divide byte # by 4 to figure out.
	load word
	sw $t0
	store word

RAM
	Text (code) ----------> .text
	foo:
	addi $t7, $t5, 2 -----> addi $t7, $t5, 2
	Data -----------------> .data

[Appendix A for ASM](https://pages.cs.wisc.edu/~larus/HP_AppA.pdf)

## MARS Demo

	