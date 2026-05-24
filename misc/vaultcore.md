# vaultcore – Writeup

## Challenge Description

> A secure vault system was recovered from an abandoned internal server.
>
> The developers claimed the protection mechanism was impossible to bypass.
>
> Can you uncover what lies inside the vault?

---

# Initial Analysis

The challenge provided a compiled binary named:

```txt
vaultcore
```

The challenge title strongly suggested:
- hidden validation logic
- password verification
- secret comparison
- or internal flag decoding routines

This indicated a reverse engineering challenge focused on understanding program behavior.

---

# Inspecting the Binary

The first step was identifying the binary type:

```bash
file vaultcore
```

Then extracting readable strings:

```bash
strings vaultcore
```

This revealed several suspicious entries:
- flag-related text
- validation messages
- hidden-looking function names
- encoded fragments

The presence of meaningful strings suggested that at least part of the validation logic was stored directly inside the binary.

---

# Static Analysis

The binary was then inspected using:
- `strings`
- `objdump`
- or similar reverse engineering tools

Important functions were identified by analyzing:
- program flow
- user input handling
- comparison logic
- hidden decoding routines

The binary appeared to:
1. accept user-controlled input
2. transform or decode internal data
3. compare the result against a hidden expected value

---

# Discovering the Hidden Logic

During analysis, the important realization was that the binary did not use strong cryptography.

Instead, it relied on:
- obfuscation
- encoded constants
- hidden validation routines
- misleading control flow

By tracing the validation path, the hidden secret used by the vault mechanism could be reconstructed.

---

# Recovering the Flag

After reversing the verification routine and reconstructing the expected value, the hidden flag was recovered successfully.
---

# Tools Used

- `file`
- `strings`
- `objdump`
- Linux terminal utilities
---

# Key Takeaway

This challenge demonstrated an important reverse engineering principle:

> Many binaries leak critical information through embedded strings and internal program structure.

The intended lesson was:
- always begin with simple static analysis
- inspect embedded strings before overcomplicating the challenge
- understand program logic before attempting brute force

Sometimes the simplest tool reveals everything needed.
