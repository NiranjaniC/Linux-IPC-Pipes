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
#include<stdio.h>

#include<stdlib.h>

#include<sys/types.h>

#include<sys/stat.h>

#include<string.h>

#include<fcntl.h>

#include<unistd.h>

#include<sys/wait.h>

void server(int,int);

void client(int,int);

int main()

{

int p1[2],p2[2],pid, *waits;

pipe(p1);

pipe(p2);

pid=fork();

if(pid==0) {

close(p1[1]);

close(p2[0]);

server(p1[0],p2[1]); return 0;

}

close(p1[0]);

close(p2[1]);

client(p1[1],p2[0]);

wait(waits);

return 0;

}

void client(int wfd,int rfd) {

int i,j,n; char fname[2000];

char buff[2000];

printf("ENTER THE FILE NAME :");

scanf("%s",fname);

printf("CLIENT SENDING THE REQUEST .... PLEASE WAIT\n");

sleep(10);

write(wfd,fname,2000);

n=read(rfd,buff,2000);

buff[n]='\0';

printf("THE RESULTS OF CLIENTS ARE ...... \n"); write(1,buff,n);

}

void server(int rfd,int wfd)

{

int i,j,n;

char fname[2000];

char buff[2000];

n=read(rfd,fname,2000);

fname[n]='\0';

int fd=open(fname,O_RDONLY);

sleep(10);

if(fd<0)

write(wfd,"can't open",9);

else

n=read(fd,buff,2000);

write(wfd,buff,n);

}




## OUTPUT
![319461317-0c1f2727-fe4a-4aa6-9925-dc1d8071eada](https://github.com/NiranjaniC/Linux-IPC-Pipes/assets/145742800/4e59f41a-5946-4398-a37e-414f147219be)



## C Program that illustrate communication between two process using named pipes using Linux API system calls
#include <unistd.h>

#include <stdlib.h>

#include <stdio.h>

#include <string.h>

int main()

{

FILE *read_fp;

char buffer[BUFSIZ + 1];

int chars_read;

memset(buffer, '\0', sizeof(buffer));

read_fp = popen("cat popen*.c | wc -l", "r");

if (read_fp != NULL) {

chars_read = fread(buffer, sizeof(char), BUFSIZ, read_fp);

}

pclose(read_fp);

exit(EXIT_SUCCESS);

}




## OUTPUT
![319462069-255f688e-9b12-42a6-bd58-db32ca37a022](https://github.com/NiranjaniC/Linux-IPC-Pipes/assets/145742800/bef0ae0a-5096-453c-9857-b1fb78ba2898)
![319462154-92534cdb-68d1-4373-accc-8fcb03e21f63](https://github.com/NiranjaniC/Linux-IPC-Pipes/assets/145742800/a0d4bd06-8bda-4ea6-933d-e13665dcf4c9)


# RESULT:
The program is executed successfully.
