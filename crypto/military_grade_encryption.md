# military_grade_encryption – Writeup

## Challenge Description

> We intercepted a suspicious encrypted transmission.  
> Analysts believe it was protected using “military grade encryption.”  
> Recover the hidden message.

---

# First Impressions

The challenge provided an encrypted text file.

The phrase:

```txt
military grade encryption
```

immediately felt a little suspicious.

In a lot of beginner and intermediate CTF challenges, dramatic names like:
- “military grade”
- “unbreakable”
- “top secret”

usually mean the challenge is trying to make the encryption sound more complicated than it actually is.

So instead of assuming real cryptography was involved, I started by checking for common encoding patterns.

---

# Looking at the Ciphertext

The encrypted file contained a long string like:

```txt
526e4a7757584a756333737759544e66655452734d325666616a526d5957...
```

At first glance, it looked like hexadecimal because:
- it only contained characters from `0-9` and `a-f`
- the structure matched common hex-encoded data.

That became the first decoding attempt.

---

# Step 1 – Decoding Hex

I decoded the hex string back into raw bytes using Python:

```python
bytes.fromhex(ciphertext)
```

The result wasn’t plaintext yet, but it clearly looked structured instead of random.

That was an important clue.

If decoding produces another readable-looking string instead of garbage, it usually means:
> there’s another layer underneath.

---

# Step 2 – Recognizing Base64

The decoded output strongly resembled Base64 because:
- it used valid Base64 characters
- it ended with padding like `=`
- and the formatting looked familiar.

So I decoded the second layer using Python:

```python
import base64

decoded = bytes.fromhex(ciphertext)
flag = base64.b64decode(decoded)

print(flag.decode())
```

This successfully revealed the hidden message.

---

# Recovering the Flag

After applying:
1. Hex decoding
2. Base64 decoding

the original flag was recovered successfully.

# What I Learned

This challenge was a good reminder that:
> encoding is not encryption.

The challenge tried to make the data look intimidating, but the actual solution relied on recognizing common encoding formats step-by-step.

Instead of jumping into complicated cryptography, the important approach was:
- identify recognizable patterns
- decode layer-by-layer
- and avoid overthinking the challenge name.
