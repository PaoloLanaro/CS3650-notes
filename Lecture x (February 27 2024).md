### Midterm on Friday March 15
Closed book, closed notes

"Compile Int \*x into something that looks like 4($sp)"
index based vs pointer based addressing

need to decompose address into (tag | index | offset)

bits can be either 0 or 1.
4 bytes can be represented with 2 bits because 2^2 is 4 so gg go next

## "Review"

fork, execvp
  fork is called once, returns twice. why?
   called from process, creates child process, and returns from both main and child process
   
  execvp is called once, returns never. why?
   creates new program for the syscall and although the main one returns execvp, the new one doesn't return anything so L? ==not sure, review== 
  
  example code w/ pipe
   read man pipe info (read end of pipe, right end of pipe)

  assembly:
  give three examples of something that might be stored in a call frame (call stack)
   1. local variables that are modified during runtime of the call. ==$ra==
   2. return address so that we can return to the actual place where the call frame is called (we want to know where to return to). ==$al==
   3. function params so that we can save it for other function calls that may happen in between. ==args, $a0, $a1==
   4. stack pointers ==$sp==

  ```
  for (i = 0; i < 10; i++) {
   x[i] = y[i];
  }

  turn it into asm

  i -> $t1
  x -> $t2
  y -> $t3
  ```

  
  labels can be used for 1) function 2) target of branch 3) variables: 
  which of these three cases would you see in .text and which inside .data segments
   variables: .data (load data / storing const)
   function, target of branch: .text (execute program)

  word in assembly might be:
  a) instruction (in .text)
  b) address
  c) data (hold data in register)

  give example using an address
   
   lw, sw, etc.

  example using data but not address

   add, sub, etc. 


different categories of assembly words:
   add, sub, etc
   beq, bne,
   jal, jr
   lw, sw