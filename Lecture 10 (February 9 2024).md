
* Symol Table
* '&' + '->' pointers
* Data structures
	* (for hw4 + hw5 (cache))

Cache pronounced "cash"
Hardware cache:

CPU has cores, cache memory
system bus between ^ v
Ram

Memory Hierarchy
Fast
register 
1/3 ns

Big
128 bytes

"clock rate"

3 Giga Hertz
3 (billion) (cycles / sec)

------
speed of light: 1 foot per nanosecond
electricity: 1 inch / ns

RAM
-\-\-\-\-\-\-
100 MHz
100 times slower than the CPU

Symbol table -> compiler, assembler, interpreter

| symbol | address |
| ---- | ---- |
| "x" | 0xa340 |
| x (label) | same thing ^ |
Symbols:
Declare it: (name + maybe type)
Allocate it (address)
\[optional] Initialize it (value)

### Pointers
int x\[5];
const int \*x = {0, 0, 0, 0, 0};

### Something else?

Suppose we have array of struct. (e.x. process table)

proc\[3].program = "ls";

synonyms
\*(proc + 3) \[programs] = "ls";
(proc + 3) -> program = "ls";

&(proc\[3]) -> program = "ls";
&: "address of"

man getdents
```
int getdents(unsigned int fd, struct linux_dirent *dirp, unsigned int count)

IN : fd
OUT : dirp
IN : count

void get_dirent(int fd, int count) {
  struct linux_direct mydirp;
  getdents(fd, &mydirp, count);
}

//==========================================
ssize_t read(int fd, void *buf, size_t count);

int mybuf[10];
void myread(int fd, int count) {
  read(fd, myBuf, count);
  read(fd, &myBuf[0], count);
}

 &mybuf[4]

homework 4:

man 2 stat
int stat(cost char *pathname, struct stat *statbuf)
mixture of both in parameters (pathname) and out parameters (for status)

ino_t st_ino inode of the file
mode_t st_mode type and mode of the file
uid_t st_uid user ID
gid_t st_gid group ID
off_t st_size size of file
```

## Homework 4
