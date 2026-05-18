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
#include <unistd.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdlib.h>
#include <stdio.h>
int main(int argc, char *argv[]) {
    if (argc != 3) {
        fprintf(stderr, "Usage: %s <source_file> <destination_file>\n", argv[0]);
        exit(EXIT_FAILURE);
    }
    char block[1024];
    int in, out;
    ssize_t nread;
    in = open(argv[1], O_RDONLY);
    if (in == -1) {
        perror("Error opening source file");
        exit(EXIT_FAILURE);
    }
    out = open(argv[2], O_WRONLY | O_CREAT | O_TRUNC, S_IRUSR | S_IWUSR);
    if (out == -1) {
        perror("Error opening destination file");
        close(in);
        exit(EXIT_FAILURE);
    }
    while ((nread = read(in, block, sizeof(block))) > 0) {
        if (write(out, block, nread) != nread) {
            perror("Error writing to destination file");
            close(in);
            close(out);
            exit(EXIT_FAILURE);
        }
    }

    if (nread == -1) {
        perror("Error reading source file");
    }
    close(in);
    close(out);
    return EXIT_SUCCESS;
}
## OUTPUT
<img width="681" height="192" alt="Screenshot 2026-05-18 094435" src="https://github.com/user-attachments/assets/71cce172-619f-490e-8ef1-4d6ec2fd4507" />
## 2.To Write a C program that illustrates files locking
import fcntl
import time
import os
filename = "filelock.c"
print(f"Opening {filename}")
fp = open(filename, "w")
fcntl.flock(fp, fcntl.LOCK_SH)
print("Acquired shared lock using flock\n")
print("Current `lslocks` output:\n")
os.system("lslocks")
time.sleep(20)
fcntl.flock(fp, fcntl.LOCK_UN)
fp.close()
## OUTPUT
<img width="645" height="345" alt="image" src="https://github.com/user-attachments/assets/13b8c9f2-a0f6-40d1-afc1-3e1bbe9ff7a6" />
# RESULT:
The programs are executed successfully.
