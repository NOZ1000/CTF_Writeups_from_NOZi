# Main idea

It is pwn task where we need to send payload that performs buffer overflow attack. We have binary elf file that we can reverse.
# Solution

To reverse the program i used Ghidra.
From decompiled main function we can see:

![](../../attachments/Pasted%20image%2020240115141509.png)

I creates buffer 64
