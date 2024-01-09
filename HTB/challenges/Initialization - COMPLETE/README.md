# Main idea 

Challenge contain `source.py`, `messages.txt`, `output.txt`. We need to decrypt encrpyted flag from `output.txt`. `messages.txt` contain 4 strings where 3 string is flag, respectivly in `output.txt` 3 line is encrypted flag.

# Reverse the code

Firstly, reading data from `messages.txt` file and storing it in `MSG` list.

![](../../../attachments/Pasted%20image%2020240109225228.png)
![](../../../attachments/Pasted%20image%2020240109225257.png)

Furthermore, `main()` creates instance of `AdvancedEncryption` class with `block_size = 128`

![](../../../attachments/Pasted%20image%2020240109224820.png)

When the instance is creates, it generates `KEYS` and `CTRs`

![](../../../attachments/Pasted%20image%2020240109224952.png)

`generate_encryption_keys`, initially creates `keys` list of length of `MSG`(in our case 4) and fills with zero bytes (this initialization of list is vulnarable)

![](../../../attachments/Pasted%20image%2020240109225100.png)
Because of unproper unproper initialization of list, the `keys` consist of 4 same values

![](../../../attachments/Pasted%20image%2020240109225658.png)

Additionally `CTRs` have the same values

![](../../../attachments/Pasted%20image%2020240109225826.png)


Then in main function 4 messages are encrypting and stores in file in hex format.

![](../../../attachments/Pasted%20image%2020240109230020.png)
It uses `encrypt(i, MSG)` method 

![](../../../attachments/Pasted%20image%2020240109230126.png)
`i` is index for each message, in this code it doesnt matter because `CTRs` and `keys` are the same. Than creates instance of AES cipher with MODE_CTR. Finally returns encrypted cipher text.

# Solution

We know that for all messages used same ctr and key. And because be know cipher texts and initial messages we can decrypt cipher that contain flag.

The formala is 

```python
c1 = m1 xor AES(k1, 0)

c2 = m2 xor AES(k2, 0)
```

Knowing `c1` and `m1` we can find `AES(k1, 0)`. And than knowing `AES(k1, 0)` ==  `AES(k2, 0)` and `c2` we can find `m2`(our flag).

```python
from pwn import xor


'''notes

c1 = m1 xor AES(k1, 0)
c2 = m2 xor AES(k2, 0)

We know c1, m2 and AES(k1, 0) == AES(k2, 0)
We can find AES(k1, 0) by xor'ing c1 and m1
Than we can find m2 by xor'ing c2 and AES(k1, 0)
'''


def main():
    m1 = b'Secret information is encrypted with Advanced Encryption Standards.'
    c1 = bytes.fromhex('2dcc93d07c4a16c833375f2b00d894c62c2d442d3cf90cd43183c559c10006372cea2c1595487c0f4314091c0c268b120f3aaabe7bd31c0c05977a7f7c4f6ce6f59392e0e522e66500e153f7a6f914c7')

    c2 = bytes.fromhex('36fdb2d97d0a5bcf0225586a1e8abfc62d3057273aab5ae5309d8c4ade060a236aed070d817b2c14110e590b1b27ef5d4d35ddc001b47d6c2bca00101c25039a')

    # find AES(k1, 0)
    aes_k1_0 = xor(c1, m1)
    m2 = xor(c2, aes_k1_0)
    print(m2)

if __name__ == '__main__':
    main()
```

https://www.hackthebox.com/achievement/challenge/995476/519
