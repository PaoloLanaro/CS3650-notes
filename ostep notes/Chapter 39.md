## 39.1 Files And Directories
- Two key abstractions have developed over time in the virtualization of storage. The first is the **file** (a linear array of bytes, each of which you can read or write).
	- Files have **low-level** names often referred to as an **inode number**. Assume each file has an inode number associated with it.
- Second abstraction is called the **directory**.
	- A directory contains a list of (user-readable name, **low-level** name) pairs.

## 39.3 Creating Files & 39.4 Reading and Writing Files
- Creating a file can be accomplished with the `open` syscall.
	- By calling `open()` and passing it the `O_CREAT` flag, a program can create a new file.
	- Example code: `int fd = open("foo", O_CREAT | O_WRONLY | O_TRUNC, S_IRUSR | S | IWUSR);`  creates a file called foo in the current working directory (cwd).
	- `open()` returns a **file descriptor**. A file descriptor is just an integer, private per process, and is used in UNIX systems to access files; thus, once a file is opened, you use the file descriptor to read or write the file, assuming you have the permissions to do so.
- File descriptors are managed by the operating system on a per-process basis. ==This means that some kind of simple structure (e.g., an array) is kept in the `proc` structure on UNIX systems.==
- ==File descriptors 0 (stdin; process can read to receive input) 1 (stdout; process can write to in order to dump information to the screen) and 2 (stderr; process can write error messages to).== Thus when you first open up another file it will almost certainly be file descriptor 3.
- The first argument to syscall `read()` is the file descriptor, thus telling the file system which file to read; a process can have multiple files open at once, so the descriptor enables the OS to know which file a particular read refers to. `read()` returns the number of bytes it read.
- Each process maintains an array of file descriptors, each of which refers to an entry in teh system-wide **open file table**. Each entry in this table tracks which underlying file the descriptor refers to, the current offset, and other relevant details such as whether the file is readable or writable.

## 39.5 Reading and Writing, But Not Sequentially
- All access so far has been sequential; that is, we have either read a file from the beginning to the end or written a file out from beginning to end.
- `lseek()` syscall allows us to be able to read or write to a specific offset within a file.
	- `off_t` lseek(int fildes, off_t offset, int whence);
	- First argument is a file descriptor as defined above.
	- Second argument is the offset, which positions the file offset to a particular location within the file.
	- Third argument determines exactly how the seek is performed 
	- "If whence is `SEEK_SET`, the offset is set to offset bytes."
	- "If whence is `SEEK_CUR`, the offset is set to its current location plus offset bytes."
	- "if whence is `SEEK_END`, the offset is set to the size of the file plus offset bytes."
	- The offset described above is kept in the `struct file` as referenced from the `struct proc`
		- Simplified version:
		- `struct file {`
		- `  int ref;`
		- `  char readable;`
		- `  char writable;`
		- `  struct inode *ip;`
		- `  uint off; };`

## 39.6 Shared File Table Entries: `fork()` And `dup()`
- There's few cases where an entry in the open file table is *shared*.
	- When a parent process creates a child process with `fork()` the entry is shared between the parent and child process so changes made in the child will be reflected on the parent.
	- Another case is the `dup()` syscall (and `dup2()` and `dup3()`). The `dup()` call is useful when writing a UNIX shell and performing operations like output redirection. 
- When a file table entry is shared, its reference count is incremented; only when both processes close the file will the entry be removed.

## 39.7 Writing Immediately With `fsync()`
- When the `fsync(int fd)` syscall is called, the file system responds by forcing all dirty (not yet written) data to disk, for the file referred to by the specified file descriptor.
- This is done because the file system usually buffers write calls for performance issues, with an eventual guarantee that bytes will be written. Sometimes this guarantee isn't good enough so `fsync()` can be used to "immediately push" the bytes.

## 39.8 Renaming Files
- the `mv` syscall takes two arguments: the original name of the file `old` and the `new` name and changes the name of the `old` file to `new`.
- A good way of creating files is by creating a tmp file (`open(file.txt.tmp, ..., ...)`) writing the new contents of that file (`write(fd, buffer, size`) force it to the disk (`fsync(fd)`) close the file (`close(fd)`), and finally rename the temp to the requested file's name (`rename("file.txt.tmp, "file.txt")`).

## 39.9 Getting Information About Files
- To see the metadata of a certain file we can use the `stat()` or `fstat()` syscalls. The syscalls take a pathname or file descriptor to a file and fill in a `stat` struct.
- Information includes, but is not limited to, the size (in bytes), its low-level name (inode #), some ownership information, and some information about when the file was accessed or modified.
- Each file system usually keeps this type of information in a structure called an `inode`

## 39.10 Removing Files

unfinished