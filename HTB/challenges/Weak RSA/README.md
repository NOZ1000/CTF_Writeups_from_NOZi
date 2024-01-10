# Main idea 

Its RSA challnage according to title. We have provided by two files `key.pub` and `flag.enc`. Exactly in `key.pub` stored public key for RSA in PEM format.

![](../../../attachments/Pasted%20image%2020240110113314.png)

It seems extremly small. And in `flag.enc` stored encrypted flag in bytes.

We can import public key into python and see what values in `n` and `e`.

```python
from Crypto.PublicKey import RSA

key = RSA.import_key(open('key.pub').read())
e = key.e
n = key.n

print(e)
print(n)
```

We can notice that `e` is unsual enormous.

![](../../../attachments/Pasted%20image%2020240110113813.png)

So lets dive into attack.

# Solution

So, i asked for Copilot to explain what if `e` is unusual large and close to `n`. 

1. **Security Risks**: If `e` is too close to `n`, it might be possible for an attacker to derive the private key `d` more easily. This is because `e` and `d` are inverses modulo `(p-1)*(q-1)`, where `p` and `q` are the prime factors of `n`. If `e` is close to `n`, and `n` is known (as it is part of the public key), then `d` might be easier to calculate, breaking the security of the RSA system.
    
2. **Wiener's Attack**: This is a classic attack on RSA when the private exponent `d` is small. If `e` is large and close to `n`, `d` might be small enough for Wiener's attack to succeed.

I googled a bit and found wieners attack implemented 