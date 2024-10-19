# Linux-IPC--Pipes
Linux-IPC-Pipes


# Ex03-Linux IPC - Pipes

# AIM:
To write a C program that illustrate communication between two process using unnamed and named pipes

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - pipe(), fifo()

### Step 3:

Testing the C Program for the desired output. 

# PROGRAM:

## C Program that illustrate communication between two process using unnamed pipes using Linux API system calls
```
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <string.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/wait.h>

void server(int rfd, int wfd);
void client(int wfd, int rfd);

int main() {
    int p1[2], p2[2], pid;
    pipe(p1);
    pipe(p2);
    pid = fork();

    if (pid == 0) {
        close(p1[1]);
        close(p2[0]);
        server(p1[0], p2[1]);
        return 0;
    }

    close(p1[0]);
    close(p2[1]);

    // Move the prompt for entering the file name here, after the fork
    printf("ENTER THE FILE NAME :\n");
    client(p1[1], p2[0]);
    wait(NULL); 
    return 0;
}

void server(int rfd, int wfd) {
    int i, j, n;
    char fname[2000];
    char buff[2000];
    n = read(rfd, fname, sizeof(fname) - 1); 
    fname[n] = '\0';
    int fd = open(fname, O_RDONLY);
    if (fd < 0) {
        write(wfd, "can't open", 9);
        return;
    }
    sleep(10);
    n = read(fd, buff, sizeof(buff) - 1); 
    buff[n] = '\0';
    write(wfd, buff, n);
    close(fd); // Close the file descriptor
}

void client(int wfd, int rfd) {
    int i, j, n;
    char fname[2000];
    char buff[2000];
    fgets(fname, sizeof(fname), stdin);
    fname[strcspn(fname, "\n")] = '\0';
    printf("CLIENT SENDING THE REQUEST .... PLEASE WAIT\n");
    sleep(10);
    write(wfd, fname, strlen(fname) + 1); 
    n = read(rfd, buff, sizeof(buff) - 1); 
    buff[n] = '\0';
    printf("THE RESULTS OF CLIENTS ARE ...... \n");
    write(1, buff, n);
}
```




## OUTPUT
![Screenshot 2024-04-05 161652](https://github.com/EzhilsreeJ/Linux-IPC-Pipes/assets/144870412/4541c14c-6347-4478-b4f4-e4fbc87a5433)


## C Program that illustrate communication between two process using named pipes using Linux API system calls
```
#include <unistd.h>
#include <stdlib.h>
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
int main(){
int res = mkfifo("/tmp/my_fifo", 0777);
if (res == 0) printf("FIFO created\n");
exit(EXIT_SUCCESS);
}
```

## OUTPUT
![Screenshot 2024-04-05 223748](https://github.com/EzhilsreeJ/Linux-IPC-Pipes/assets/144870412/60b4c57d-5f3a-4080-860e-c3c81e3ddc5f)


# RESULT:
The program is executed successfully.
