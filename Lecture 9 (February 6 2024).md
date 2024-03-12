## Makefile

As before, you must submit a "tarball".  It must include a Makefile
and a README file.  You will include 'hw3.asm'.  Please do _not_
include Mars4_5.jar in your tarball.  Let's keep the tarballs small.

One of the targets of your Makefile should be
${FILE}.tar.gz, just as you did in hw1 and hw2.  As before, you will
use the 'submit' script for hw3 to submit.

Finally, your Makefile must include a 'check' target.  The graders
will run 'make check' on your program to verify that it works.
Your 'check' target will compare the output of your assembly program
with the output of collatz.py.  The Linux 'diff' command compares
the two outputs.

You will need the following targets.  (Don't forget that the Makefile
command after the target must begin with a `<TAB>` character.)

On Khoury login, it's possible to run Mars from the command line.
I suggest debugging your hw3.asm locally with the full Mars GUI.
You can then verify with 'make check' either on your local computer
or on Khoury login.  (But the graders will grade, based on Khoury login.)
For 'make check' on Khoury, you can copy from the course dir to your hw3 dir:
  cp /course/cs3650/MIPS-simulator/Mars4_5.jar ./

\# The command 'tail -n +3', below, is a 'filter' command.  It omits the first
\# two lines, since that is the banner for Mars itself.
hw3.out: hw3.asm Mars4_5.jar
	java -jar Mars4_5.jar hw3.asm | tail -n +3 > hw3.out

collatz.out: collatz.py
	python3 collatz.py > collatz.out

check: collatz.out hw3.out
	diff collatz.out hw3.out

And of course, you must include a standard 'clean' and 'dist' target:

\# Note that this will remove your 'Mars4_5.jar'.  Keep an extra copy elsewhere.
clean:
	rm -f collatz.out hw3.out Mars4_5.jar

dist: clean
	dir=`basename $$PWD`; cd ..; tar czvf \$\$dir.tar.gz ./\$\$dir
	dir=`basename $$PWD`; ls -l ../\$$dir.tar.gz

## Class

C language, or HLL
Assembly
O/S Kernel

Process table
file descriptor 0 stdin
file descriptor 1 stdout
file descriptor 3 stderr

I/O redirection
	ls > output
	binds the stdout of ls to a file descriptor for output and then will send it to output
	ls | wc

Assembly: syscall is whatever you decide to put in $v0 so li $v0, 5 means execute system call 5

libc.so
libc.a

Extended our readings for this week YIPEE
![[YIPEE.png]]
[link to textbook](https://pages.cs.wisc.edu/~remzi/OSTEP/)
chapters 39 and 40 were added

[unix-xv6 sys_open](https://course.ccs.neu.edu/cs3650/unix-xv6/HTML/S/7.html#L284)
[unix-xv6 struct file](https://course.ccs.neu.edu/cs3650/unix-xv6/HTML/S/21.html)

man open

`int open(const char \*pathname, int flags, mode_t mode);`

lots of structs leading to lots of objects / things is the main idea 
review: File system implementation on the main textbook
