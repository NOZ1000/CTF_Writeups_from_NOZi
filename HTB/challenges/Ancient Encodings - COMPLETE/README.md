# Main idea

Easy challenge encoding flag with base64, than converting bytes to integer, converting to hex

![](../../../attachments/Pasted%20image%2020240109101602.png)

# Solution

```python
from  Crypto.Util.number import long_to_bytes
from base64 import b64decode

output_hex = 0x53465243657a51784d56383361444e664d32356a4d475178626a6c664e44497a5832677a4d6a4e664e7a42664e5463306558303d

def decode():
    return b64decode(long_to_bytes(output_hex))


print(decode())
```

> [!note] 
> `output_hex` = `0x5346...` coverts to integer immidiatly

