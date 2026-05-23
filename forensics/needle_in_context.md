# needle_in_context – Writeup

## Challenge Description

> During a failed forensic recovery attempt, several debug logs were partially reconstructed.
>
> Investigators believe some entries may still contain fragments of sensitive data.
>
> Be warned: not every recovery artifact is trustworthy.

Flag format:

```txt
SecLeaf{}
```

---

# Initial Analysis

After extracting the provided log archive, the directory contained hundreds of log files with repetitive entries such as:

```txt
SecLeaf{temporary_15899}
```

These appeared repeatedly across multiple files.

The repetition, combined with the keyword:

```txt
temporary
```

strongly suggested that these were intentional decoy flags rather than the real solution.

Instead of brute-forcing every candidate, I shifted focus toward:
- anomalies
- unusual filenames
- non-repetitive patterns

---

# Identifying Suspicious Files

Among the large number of generic log files, several filenames stood out immediately:

```txt
notes2.log
notes3.log
notes4.log
final_77.log
recovered_8.log
archive_150.log
```

These differed from the normal naming pattern and appeared semantically meaningful.

This suggested that the important clues were likely distributed across these files.

---

# Discovering Hexadecimal Fragments

Inside the suspicious log files, I found entries such as:

```txt
636f6e74
6578745f
68655f72
```

These values:
- contained only hexadecimal characters
- had even lengths
- resembled ASCII hexadecimal encoding

This strongly suggested that the logs contained fragmented hexadecimal-encoded text.

---

# Decoding the Fragments

I decoded the fragments using:

```bash
echo 636f6e74 | xxd -r -p
```

Output:

```txt
cont
```

Additional fragments decoded into:

| Hex | ASCII |
|---|---|
| 636f6e74 | cont |
| 6578745f | ext_ |
| 68655f72 | he_r |

At this point, it became clear that the challenge involved reconstructing fragmented text from multiple logs.

---

# Searching for Additional Fragments

To locate all hexadecimal fragments inside the logs, I used:

```bash
grep -R "[0-9a-f]\{8\}" logs/
```

This revealed additional fragments:

```txt
5365634c
6561667b
69735f74
65616c5f656e656d797d
```

After decoding:

| Hex | ASCII |
|---|---|
| 5365634c | SecL |
| 6561667b | eaf{ |
| 69735f74 | is_t |
| 65616c5f656e656d797d | eal_enemy} |

---


# Key Takeaways

This challenge demonstrated:
- forensic log analysis
- anomaly detection
- hexadecimal pattern recognition

The challenge rewarded careful filtering and pattern recognition instead of brute-force searching.
