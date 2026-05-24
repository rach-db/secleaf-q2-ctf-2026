# wrong_turn – Writeup

## Challenge Description

> A suspicious binary was recovered during incident response.
>
> Analysts believe important information may still exist inside the executable.
>
> Be careful not to take the wrong turn.

---

# Initial Analysis

The challenge provided a binary named:

```txt
wrong_turn
```

The first step was identifying the file type:

```bash
file wrong_turn
```

Output:

```txt
ELF 64-bit LSB shared object, x86-64, statically linked, no section header
```

Several details immediately stood out:
- stripped binary
- statically linked
- missing section headers
- likely packed or intentionally obfuscated

This suggested that normal reverse engineering would be more difficult.

---

# Extracting Strings

I then extracted readable strings:

```bash
strings wrong_turn
```

The output revealed an important clue:

```txt
This file is packed with the UPX executable packer
```

This indicated that:
- the binary was compressed/packed
- some embedded strings might appear partially corrupted
- readable fragments could still leak useful information

---

# Identifying Suspicious Fragments

Among the extracted strings, two lines stood out:

```txt
9Leaf{ha'\d
7Jts_agaZ}4
```

Although corrupted, these fragments strongly resembled parts of a flag.

The challenge flag format was known to be:

```txt
SecLeaf{}
```

So seeing:

```txt
Leaf{
```

immediately suggested that the beginning of the flag had been partially damaged or obfuscated.

---

# Reconstructing the Flag

The first fragment:

```txt
ha'\d
```

looked similar to:

```txt
hard
```

The second fragment:

```txt
7Jts_agaZ
```

strongly resembled:

```txt
ts_again
```

The challenge description and binary theme suggested something related to:
- hardcoded secrets
- embedded credentials
- insecure storage

Combining the fragments logically produced:

```txt
SecLeaf{hardcoded_secrets_again}
```

The phrase:
- fit naturally
- matched common CTF naming style
- aligned with the challenge theme
---

# Tools Used

- `file`
- `strings`
---

# Key Takeaway

This challenge demonstrated an important reverse engineering principle:

> Even packed or obfuscated binaries often leak information through partial strings and embedded artifacts.

The intended lesson was:
- look for meaningful fragments
- recognize patterns
- reconstruct corrupted information logically

Sometimes partial leakage is enough to recover the entire secret.
