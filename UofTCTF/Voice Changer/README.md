# Main idea

Web task with simple functionality, where we can edit pitch of our voice.

![](../../attachments/Pasted%20image%2020240115131800.png)

When we click submit it send POST request

![](../../attachments/Pasted%20image%2020240115132204.png)

In output we can see that this app executes command in bash.

# Solution

We can submit form and intercept it with Burp:

![](../../attachments/Pasted%20image%2020240115132109.png)

We see our pitch value and lets try modify it to gain RCE

![](../../attachments/Pasted%20image%2020240115132416.png)

In output we can see our payload:

![](../../attachments/Pasted%20image%2020240115132448.png)

Lets try to bypass quotes in command and execute our command:

![](../../attachments/Pasted%20image%2020240115132555.png)

Now we can see files from apps directory:

![](../../attachments/Pasted%20image%2020240115132640.png)

We can search flag:

![](../../attachments/Pasted%20image%2020240115132925.png)

And finally cat it.

```sh
"; cat /flag.txt;
```
