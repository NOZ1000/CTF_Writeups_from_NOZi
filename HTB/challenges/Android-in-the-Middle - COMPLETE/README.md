# Main idea

Program takes two input 

1) The Public Key of The Memory: `Must be an integer`
2) The Encrypted Initialization Sequence: `Must be (% 16) len hex`


# Reverse sever's code

To solve challenge we need to we need to get `sequence == b"Initialization Sequence - Code 0"`

![](../../../attachments/Pasted%20image%2020240108184909.png)
Sequence generates with function `decrypt`, that takes two inputs

![](../../../attachments/Pasted%20image%2020240108184929.png)

`encrypted_sequnce`

![](../../../attachments/Pasted%20image%2020240108184945.png)

`shared_secret`

![](../../../attachments/Pasted%20image%2020240108185005.png)

`shared_secret` genrates by formula `pow(M, c, p)`, we know `M`(public key from input) and `p`(gived initially). We dont know `c`, because it is generated randomly.

![](../../../attachments/Pasted%20image%2020240108185025.png)

From 

![](../../../attachments/Pasted%20image%2020240108185039.png)

We can brutoforce and find `c`, because `shared_secret`(out input), `M`(our input), `p`(initiialy known)

> [!important] 
> If we use M Public key as 1, than we dont need to guess what value of `c` is.
> 



To get the flag we need in returning `message` get `b"Initialization Sequence - Code 0"`

![](../../../attachments/Pasted%20image%2020240108185055.png)

# Solution

I have wrote encrypting [function](Script.md)  

![](../../../attachments/Pasted%20image%2020240108185108.png)

Now we can play with values 

![](../../../attachments/Pasted%20image%2020240108185126.png)

With `M = 1`  `shared_secret` always will be `1`, so we can encrypt message with this key, and now we know we `ecrypted` message with this key. That means we can pass to the server `M  = 1`, `encrypted_sequence = 7fd4794e77290bf65808e95467f284966d71995c16e83da2192aecfd2d0df7a4` (encrypted `b"Initialization Sequence - Code 0"` with key `1`)


Boom! We get the flag.

![](../../../attachments/Pasted%20image%2020240108185150.png)

https://www.hackthebox.com/achievement/challenge/995476/340

