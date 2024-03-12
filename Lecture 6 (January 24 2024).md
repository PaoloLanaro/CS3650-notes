## Homework 2 Questions

- Adding "record.out>" for the verbose flag.
	- program filter is reading from the pipe. it's getting a stream of the chars.
```python
'record.out>' + str.replace('\n', '\n' + 'record.out')
```
```c
read(0, buf, 1)
// --->
char read_one() {
  char buf[1];
  read(0, buf, 1);
  return buf[1];
}
// char outfile[1024]; // hope we never see 1000 char
/* OR: */ write(1, buf, 1); // which write one char at a time.

write_one_str(char *);
```
Converting the python program to C
```c
write_one_str("record.out> ");
char x;
for (x = read_one_char; x == end.of.file;) {
  write_one_char(x);
  if (x == '\n') {
    write_one_str("record.out> ");
  }
}
```

- man 2 open
- arguments for the flag. (aka whether it's an input or output file descriptor)
- O_Creat
- O_Trunc

- S_IRUSR | S_IWUSR
- read perms, and write perms.
- 
![[hw 2 sh.c xv6 pipe.png]]

### Prof comments


```
In hw2, Case B, you should end up wiwth the following internals:

  'ls -l' -> stdout -> pipe -> stdin -> './filter'

If you're doing that, then the 'ls -l' process will exit. Then stdout shows an end-of-file. Then the pipe is removed. And then, stdin will show an end-of-file. Then, When 'filter' reads stdin using 'read(0, buf, 1)', it returns 0 (0 chars read), which means end-of-file.
read returns 0 means it read 0 characters. Test that the return value is 0, then you know the filter program is done.

*-*-* Suggestion *-*-*

// sleep(2);
waitpid(childpid);
man 2 waitpid

pid_t waitpid(pid_t pid, int *wstatus, int options);

ctrl f wstatus in the man page for waitpid.
if wstatus is not NULL, waitpid() store status information.
// make it NULL

ctrl f options in the man page for waitpid.
value of option is an OR of zero or more of the following
// make it 0

waitpid(childpid, NULL, 0);
```

RAM (memory)

Process

code

----
data segment

----

----
stack / call frames

----
stack / call frames

----
stack / call frames



