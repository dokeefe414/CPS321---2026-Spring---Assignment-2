# How write() Becomes a System Call in Linux 0.11

When a C program calls write(fd, buf, count), it does not go directly into the kernel.

First, the call goes to the libc version of write(). This is a wrapper function that prepares a system call. It places the system call number (4 for write) into the EAX register, and the arguments into EBX, ECX, and EDX.

Next, libc executes the instruction `int 0x80`. This is a software interrupt that switches the CPU from user mode to kernel mode.

The kernel receives control in a function called system_call. This function checks the system call number and uses it as an index into the sys_call_table.

Since the number is 4, it calls sys_write().

The sys_write() function runs inside the kernel and performs the requested operation.

After it finishes, the result is placed back into EAX, and control returns to the user program.
