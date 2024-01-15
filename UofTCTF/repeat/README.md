# Main idea

It is easy crypto task, where we have two files `flag.enc` and `gen.py`. In `flag.enc` we can see output of `gen.py` in hex format.

![](../../attachments/Pasted%20image%2020240115134918.png)

# Reverse code 

1. Generating random 8 bytes  and store it to `xor_key`
2. Converting `flag` to bytes.
3. Xoring `flag` with random 8 bytes `xor_key`
4. Covert result of xor to hex format
5. Writing `encrypted_flag` to file

# Solution

To solve this task we need to know key `xor_key` to reverse this operations and retrieve original flag. Before solving we need to understand how xor works.

