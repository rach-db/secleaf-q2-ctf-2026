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

# First Impressions

The challenge provided a single file:

```txt
encrypted.txt
```

I started by checking its contents:

```bash
cat encrypted.txt
```

Output:

```txt
526e4a7757584a756333737759544e66655452734d325666616a526d5957
64664d3245776148523166513d3d0a
```

The string only contained hexadecimal characters (`0-9`, `a-f`), which immediately suggested that the first layer was probably hex encoding.

The challenge title:

```txt
Double Trouble
```

also hinted that this wouldn’t be just a single decoding step.

---

# Step 1 – Decoding the Hex

I decoded the hex string using:

```bash
echo 526e4a7757584a756333737759544e66655452734d325666616a526d595764664d3245776148523166513d3d0a | xxd -r -p
```

Output:

```txt
RnJwWXJuc3swYTNfeTRsM2VfajRmYWdfM2EwaHR1fQ==
```

This looked very familiar.

A few things immediately stood out:
- valid Base64 character set
- trailing `==`
- overall Base64 formatting pattern

So the next layer was almost certainly Base64.

---

# Step 2 – Decoding Base64

I decoded the Base64 string:

```bash
echo RnJwWXJuc3swYTNfeTRsM2VfajRmYWdfM2EwaHR1fQ== | base64 -d
```

Output:

```txt
FrpYrns{0a3_y4l3e_j4fag_3a0htu}
```

At first glance, this still didn’t look correct.

But it also didn’t look random.

That was the important clue.

The structure still resembled a flag:
- same braces
- readable character distribution
- partially recognizable words

So clearly, another transformation layer still remained.

---

# Step 3 – Recognizing ROT13

The beginning of the string gave away the trick:

```txt
Frp
```

Applying ROT13 to that produces:

```txt
Sec
```

That immediately confirmed the final encoding layer.

I applied ROT13 using:

```bash
echo 'FrpYrns{0a3_y4l3e_j4fag_3a0htu}' | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

Output:

```txt
SecLeaf{0n3_l4y3r_w4snt_3n0ugh}
```

And that turned out to be the correct flag.

---

# What I Learned

This challenge was a really good example of layered encoding.

The important part wasn’t using advanced cryptography — it was recognizing patterns step-by-step and not stopping after the first successful decode.

The solve path became:

1. Hex decode
2. Base64 decode
3. ROT13 decode

What I liked about this challenge was that each layer naturally hinted toward the next one instead of feeling random or brute-force based.
