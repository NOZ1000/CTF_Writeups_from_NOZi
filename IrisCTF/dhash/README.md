# Main Idea

This challenge outputs 2 numbers. N and e(always 65537).
Than waits for input(hex string that length is devides to 256).
Our goal is to generate payoad that after proceessing by server outputs the hash that is zero hexstring(00 * 256)


# Reverse the code

1) Just creates hash object, waits for user input,  validates the input
   
![image](https://github.com/NOZ1000/CTF_Writeups_from_NOZi/assets/56728939/4a2ccda1-f185-4467-a9fb-74467ac77503)

3) Generates the hash, than checks if hash is fully zero sends the flag
   
![image](https://github.com/NOZ1000/CTF_Writeups_from_NOZi/assets/56728939/cfa12adc-34d5-490c-be51-ea4ce7770797)

## Lets reverse the hash class and its methods that generates the hash

4) Firstly it devides our input srting to blocks containing 256 bytes

![image](https://github.com/NOZ1000/CTF_Writeups_from_NOZi/assets/56728939/d29c2c93-a546-45ce-a532-bc339dbf7fc4)

5) Then simply validates that this block is not even seen here, also converts the bytes to integer, than generates data by modular exponentiation operation pow(data, self.e, self.N), than again converts to bytes and returned to next operation

![image](https://github.com/NOZ1000/CTF_Writeups_from_NOZi/assets/56728939/115a97fa-4b3d-4cf5-bb3a-f43728cc674e)

6) Next operation is custom xor function, it just xors current block with current _state(class attribute itially it is 00 * 256). Then xored value return to current _state. And loops over all blocks provided from input.

![image](https://github.com/NOZ1000/CTF_Writeups_from_NOZi/assets/56728939/059e4edd-5fa8-4e9a-919e-41e307eae279)

![image](https://github.com/NOZ1000/CTF_Writeups_from_NOZi/assets/56728939/431b25d3-6677-48a6-9bdd-7751bd6c0474)

# Conclusion of reverse

After reversing the code we can now understand that main steps
1) Devides our input into blocks by 256 bytes
2) Each block calculates modular exponentiation than converts to bytes and xores with previous state and stores it to state

# Solution

Knowing that we can generate payload with three blocks, that after xor operations generate zero hash

I have written script that generate three blocks, third block is xored value of first two. And thats it

![image](https://github.com/NOZ1000/CTF_Writeups_from_NOZi/assets/56728939/23beff93-b22e-4faf-9fba-87d8344b0b1c)

Also one steps, we cannot upload this payload because we know that our payload goes throw modular exponentiation operation, so we can predict that by function

![image](https://github.com/NOZ1000/CTF_Writeups_from_NOZi/assets/56728939/10e6eb16-80b4-42e5-ae72-8326ac0cf173)

Also in input we need to separate out bytes in hex format 

![image](https://github.com/NOZ1000/CTF_Writeups_from_NOZi/assets/56728939/4e329568-7668-48dc-bda9-c89b9935793a)

An thats it, save payload to file

![image](https://github.com/NOZ1000/CTF_Writeups_from_NOZi/assets/56728939/2df70858-2039-4959-958f-d7f9d7d53a7b)

Boom! Get the flag

![image](https://github.com/NOZ1000/CTF_Writeups_from_NOZi/assets/56728939/4744708c-9685-4d82-b94b-5cfb307f5a39)
