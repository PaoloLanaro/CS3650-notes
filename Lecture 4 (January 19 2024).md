## HW1 Questions

Read first char.
- '#'
- '1'
- '2'

read(0, buf, 1)
- man 2 read
0 = stdin
1 = 1 character to be read.

``` c
char buf[1]; // basically an array
read(0, buf, 1); 
while (*buf == '#') { // first element of the array buf. can also be done buf[0]
	read(0, buf, 1); // kinda unnecessary
	while (buf[0] != '\n') { // same thing as *buf
	read(0, buf, 1);
	}
}
```
## HW2

### Case A


### Case B

man 2 open
open("pathname", FLAGS, MODE);
- MODE is essentially perms like rwx
Will return a file descriptor, not 0-2, they're open (busy) to the terminal.
Probably fd == 4;

man 2 pipe

record | filter  

(stdout: pipe) (pipe :stdin which filters)