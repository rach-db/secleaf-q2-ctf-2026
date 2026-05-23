
# military_grade_encryption - Writeup

## Challenge Description

> We intercepted a suspicious encrypted transmission.  
> Analysts believe it was protected using “military grade encryption.”  
> Recover the hidden message.

---

# Initial Analysis

The challenge provided an encrypted text file.

The wording “military grade encryption” suggested that the challenge was likely trying to intimidate players into overthinking the solution.

In many beginner-to-intermediate CTF crypto challenges, phrases like:
- “military grade”
- “secure encryption”
- “unbreakable cipher”

usually indicate:
- layered encodings
- weak obfuscation
- or simple transformations hidden behind dramatic wording.

---

# Inspecting the Ciphertext

The encrypted file contained a long encoded-looking string.

Example structure:

```txt
526e4a7757584a756333737759544e66655452734d325666616a526d5957...
```

At first glance, the data looked hexadecimal.

---

# First Decoding Layer — Hex

The first step was converting the hexadecimal string back into readable bytes.

Using Python:

```python
bytes.fromhex(ciphertext)
```

After decoding from hex, the output became another encoded string rather than plaintext.

This immediately suggested:
- the encryption was layered
- multiple transformations had been applied sequentially

---

# Second Decoding Layer — Base64

The resulting decoded text strongly resembled Base64 because:
- it used only valid Base64 characters
- it ended with padding characters like `=`

Decoding the Base64 layer revealed the hidden message.

Using Python:

```python
import base64

decoded = bytes.fromhex(ciphertext)
flag = base64.b64decode(decoded)

print(flag.decode())
```

---

# Recovering the Flag

After applying:
1. Hex decoding
2. Base64 decoding

the original message was recovered successfully.
