
# Main idea

We have script that encrypts `flag`(redacted in script), and have output `p`, `g`, `h`, `c1` and `c2`.

We need to reverse the code to write solution

# Reverse code

The `main` function just generate `p`, `g`, `h`, `c1` and `c2` than saves them to out.txt

![](../../../attachments/Pasted%20image%2020240109093819.png)

The `gen_params` function generates `p`, `x`, `h`

![](../../../attachments/Pasted%20image%2020240109093934.png)

The `encrypt(pubkey)` function takes `p`, `g`, `h` and returns `c1`, `c2`. `m` is our flag that we need decrypt.

![](../../../attachments/Pasted%20image%2020240109094039.png)

As i mentiont before we have `p`, `g`, `h`, `c1` and `c2` . Knowing that we can find variables  `y` and `s` from equations bellow

`c1 = g * y % p`        =>    `y = c1 * inverse(g, p) % p`

`s = pow(h, y, p)`    =>    `s = pow(h, y, p)`

`c2 = m * s % p`        =>    `m = c2 * inverse(s, p) % p`

# Solution script

```python

```

