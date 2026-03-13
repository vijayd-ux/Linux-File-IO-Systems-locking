# Linux-File-IO-Systems-locking
Ex07-Linux File-IO Systems-locking
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
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *source, *dest;
    char ch;

    source = fopen("source.txt", "r");
    if (source == NULL) {
        printf("Source file not found!\n");
        return 1;
    }

    dest = fopen("destination.txt", "w");
    if (dest == NULL) {
        printf("Cannot create destination file!\n");
        fclose(source);
        return 1;
    }

    while ((ch = fgetc(source)) != EOF) {
        fputc(ch, dest);
    }

    printf("File copied successfully!\n");

    fclose(source);
    fclose(dest);

    return 0;
}
```


## 2.To Write a C program that illustrates files locking
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include <string.h>

int main() {
    int fd;
    struct flock lock;

    fd = open("locked.txt", O_RDWR | O_CREAT, 0644);
    if (fd < 0) {
        printf("Error opening file!\n");
        return 1;
    }

    // Set write lock
    lock.l_type = F_WRLCK;
    lock.l_whence = SEEK_SET;
    lock.l_start = 0;
    lock.l_len = 0;
    lock.l_pid = getpid();

    printf("Locking file...\n");

    if (fcntl(fd, F_SETLK, &lock) == -1) {
        printf("File already locked by another process!\n");
        close(fd);
        return 1;
    }

    printf("File locked successfully!\n");

    // Write to file
    write(fd, "This is locked content", 23);
    printf("Data written to file\n");

    printf("Press Enter to unlock...");
    getchar();

    // Unlock the file
    lock.l_type = F_UNLCK;
    fcntl(fd, F_SETLK, &lock);
    printf("File unlocked!\n");

    close(fd);
    return 0;
}
```


## OUTPUT
```
[vijay@parrot]-[~/os/ex07/Linux-File-IO-Systems-locking]
$ ./copy
File copied successfully!

[vijay@parrot]-[~/os/ex07/Linux-File-IO-Systems-locking]
$ cat destination.txt
Hello Vijay

┌─[✗]─[vijay@parrot]─[~]
└──╼ $vi test.c
┌─[vijay@parrot]─[~]
└──╼ $gcc test.c -o test.o
┌─[vijay@parrot]─[~]
└──╼ $./test.o
Locking file...
File locked successfully!
Data written to file
Press Enter to unlock...
File unlocked!
```



# RESULT:
The programs are executed successfully.
