
# Double Trouble – Writeup

## Challenge Description

> We intercepted a suspicious encoded transmission during routine monitoring.
>
> Analysts believe the message was processed through multiple transformation layers before being transmitted.
>
> Can you recover the original message?

Flag Format:

```txt
SecLeaf{}
```

---

# Initial Analysis

The challenge provided a single file:

```txt
encrypted.txt
```

Viewing the contents:

```bash
cat encrypted.txt
```

Output:

```txt
526e4a7757584a756333737759544e66655452734d325666616a526d5957
64664d3245776148523166513d3d0a
```

The string contained only hexadecimal characters (`0-9`, `a-f`), which strongly suggested the first encoding layer was hexadecimal.

The challenge title **Double Trouble** also hinted that multiple decoding steps would be required.

---

# Step 1 – Decode Hexadecimal

I decoded the hex string using `xxd`:

```bash
echo 526e4a7757584a756333737759544e66655452734d325666616a526d595764664d3245776148523166513d3d0a | xxd -r -p
```

Output:

```txt
RnJwWXJuc3swYTNfeTRsM2VfajRmYWdfM2EwaHR1fQ==
```

This output clearly resembled Base64 because:

- it used valid Base64 characters
- it ended with `==`
- the structure matched common Base64 formatting

---

# Step 2 – Decode Base64

Next, I decoded the Base64 string:

```bash
echo RnJwWXJuc3swYTNfeTRsM2VfajRmYWdfM2EwaHR1fQ== | base64 -d
```

Output:

```txt
FrpYrns{0a3_y4l3e_j4fag_3a0htu}
```

At first glance, this still looked incorrect however the challenge title suggested another transformation layer still remained.

---

# Step 3 – Identify ROT13 Encoding

The partially readable output strongly resembled ROT13-obfuscated text.

For example:

```txt
Frp
```

decodes to:

```txt
Sec
```

using ROT13.

I applied ROT13 using:

```bash
echo 'FrpYrns{0a3_y4l3e_j4fag_3a0htu}' | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

Output:

```txt
SecLeaf{0n3_l4y3r_w4snt_3n0ugh}
```

---

# Final Flag

```txt
SecLeaf{0n3_l4y3r_w4snt_3n0ugh}
```

---

# Key Takeaway

The challenge relied on recognizing layered encodings:

1. Hexadecimal
2. Base64
3. ROT13
