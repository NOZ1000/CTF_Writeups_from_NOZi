# Main idea 

Challenge contain `source.py`, `messages.txt`, `output.txt`

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


