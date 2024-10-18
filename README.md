# Linux-File-IO-Systems-locking
# AIM:
To Write a C program that illustrates files copying and locking

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux IO Systems locking

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## 1.To Write a C program that illustrates files copying 
```
#include <unistd.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdlib.h>
int main()
{
char block[1024];
int in, out;
int nread;
in = open("filecopy.c", O_RDONLY);
out = open("file.out", O_WRONLY|O_CREAT, S_IRUSR|S_IWUSR);
while((nread = read(in,block,sizeof(block))) > 0)
write(out,block,nread);
exit(0);}

```
## 2.To Write a C program that illustrates files locking
```

#include <fcntl.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <sys/file.h>
int main (int argc, char* argv[])
{ char* file = argv[1];
 int fd;
 struct flock lock;
 printf ("opening %s\n", file);
 /* Open a file descriptor to the file. */
 fd = open (file, O_WRONLY);
// acquire shared lock
if (flock(fd, LOCK_SH) == -1) {
    printf("error");
}else
{printf("Acquiring shared lock using flock");
}
getchar();
// non-atomically upgrade to exclusive lock
// do it in non-blocking mode, i.e. fail if can't upgrade immediately
if (flock(fd, LOCK_EX | LOCK_NB) == -1) {
    printf("error");
}else
{printf("Acquiring exclusive lock using flock");}
getchar();
// release lock
// lock is also released automatically when close() is called or process exits
if (flock(fd, LOCK_UN) == -1) {
    printf("error");
}else{
printf("unlocking");
}
getchar();
close (fd);
return 0;
}

```

## OUTPUT
## Files copying:
```
Nishadayalan@Nisha-laptop:~$ gcc -o filecopy1.o filecopy1.c
Nishadayalan@Nisha-laptop:~$ ./filecopy1.o
Nishadayalan@Nisha-laptop:~$ ls -l filecopy.o
-rwxr-xr-x 1 Nishadayalan Nishadayalan 16088 Oct 10 08:07 filecopy.o
Nishadayalan@Nisha-laptop:~$ diff filecopy1.c filecopy.c
Nishadayalan@Nisha-laptop:~$

```
## Files Locking:
```
Nishadayalan@Nisha-laptop:~$ vi lock.c
Nishadayalan@Nisha-laptop:~$ gcc -o lock.o lock.c
Nishadayalan@Nisha-laptop:~$ vi tricky.txt
Nishadayalan@Nisha-laptop:~$ ./lock.o tricky.txt
opening tricky.txt
Acquiring shared lock using flock
Acquiring exclusive lock using flock
unlocking
Nishadayalan@Nisha-laptop:~$ lslocks
COMMAND         PID  TYPE SIZE MODE  M START END PATH
(unknown)        54 FLOCK      WRITE 0     0   0 /run...
```
# RESULT:
The programs are executed successfully.
