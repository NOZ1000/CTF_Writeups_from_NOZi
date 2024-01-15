# Main idea

It is easy crypto task, where we have two files `flag.enc` and `gen.py`. In `flag.enc` we can see output of `gen.py` in hex format.

![](../../attachments/Pasted%20image%2020240115134918.png)

# Reverse code 

1. Generating random 8 bytes  and store it to `xor_key`
2. Converting `flag` to bytes.
3. Xoring `flag` with random 8 bytes `xor_key`
4. Covert result of xor to hex format
5. Writing `encrypted_flag` to file

Breaking down xor function:

`xor` that takes two arguments: `message` and `key`. This function is used to perform an XOR operation on the `message` using the `key`. XOR, or exclusive OR, is a binary operation that takes two bits and returns 1 if exactly one of the bits is 1, otherwise it returns 0.

The function uses a list comprehension to iterate over the length of the `message`. For each index `i` in the `message`, it performs an XOR operation between the `i`th byte of the `message` and the `i`th byte of the `key`. If the length of the `key` is less than the length of the `message`, the key is reused in a cyclic manner. This is achieved by using the modulo operator `%` with the length of the `key` to wrap around the `key` index.

fakeflag{fake_flag}
      XOR
keykeykeykeykeykey
      =
someencrypteddata

# Solution

To solve this task we need to know key `xor_key` to reverse XOR operation and retrieve original flag.

`encrypted_flag` XOR `xor_key` = `flag`

We know `encrypted_flag` and dont know `xor_key`.
But we can predict key, because we can predict the begining of the flag.

`encrypted_flag` XOR `xor_key` = uoftctf{ + end_of_flag

prefix uoftctf{ is 8 bytes long and `xor_key` 8 bytes long, so

`encrypted_flag`(first 8 bytes) XOR  uoftctf{ = `xor_key`

and we can retrieve flag
# Solution script 

```python
from Crypto.Util.number import long_to_bytes

enc_flag = '982a9290d6d4bf88957586bbdcda8681de33c796c691bb9fde1a83d582c886988375838aead0e8c7dc2bc3d7cd97a4'
enc_flag = bytes.fromhex(enc_flag)

def xor(message, key):
    return bytes([message[i] ^ key[i % len(key)] for i in range(len(message))])

def find_key(encrypted_flag):
    signature = b'uoftctf{'
    key = xor(encrypted_flag[:len(signature)], signature)
    
    return key

key = find_key(enc_flag)

flag = xor(enc_flag, key)
print(flag.decode())

```

![](../../attachments/Pasted%20image%2020240115140456.png)
