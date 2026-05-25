# wrong_turn – Writeup

## Challenge Description

> A suspicious binary was recovered during incident response.
>
> Analysts believe important information may still exist inside the executable.
>
> Be careful not to take the wrong turn.

---

# First Impressions

The challenge provided a binary named:

```txt
wrong_turn
```

The name itself suggested some kind of misdirection or trap, so I expected the challenge to contain misleading artifacts or intentionally confusing output.

The first thing I did was identify the binary:

```bash
file wrong_turn
```

Output:

```txt
ELF 64-bit LSB shared object, x86-64, statically linked, no section header
```

A few things immediately stood out:
- stripped binary
- statically linked
- missing section headers

That usually means the binary is intentionally made harder to reverse and may also be packed or obfuscated.

---

# Extracting Strings

Instead of jumping straight into heavy reverse engineering, I started simple:

```bash
strings wrong_turn
```

This turned out to be the most useful step.

One line immediately caught my attention:

```txt
This file is packed with the UPX executable packer
```

That explained why some of the output looked corrupted or incomplete.

Packed binaries often still leak fragments of important data through partially recoverable strings.

---

# Looking for Meaningful Fragments

While scrolling through the extracted strings, these two fragments stood out:

```txt
9Leaf{ha'\d
7Jts_agaZ}4
```

At first glance they looked broken, but they also looked suspiciously close to a flag.

Since the challenge format was known to be:

```txt
SecLeaf{}
```

seeing:

```txt
Leaf{
```

was already a huge clue.

At that point, the challenge became more about reconstructing damaged text logically rather than fully reversing the binary.

---

# Reconstructing the Flag

The fragment:

```txt
ha'\d
```

looked very similar to:

```txt
hard
```

And:

```txt
7Jts_agaZ
```

strongly resembled:

```txt
ts_again
```

Now the phrase started forming naturally.

Given the challenge theme and common CTF wording, the most reasonable reconstruction became:

```txt
SecLeaf{hardcoded_secrets_again}
```

The phrase fit perfectly:
- semantically
- stylistically
- and contextually with the challenge theme.

---

# Tools Used

- `file`
- `strings`

---

# What I Learned

This challenge was a good reminder that not every reverse engineering problem requires deep disassembly.

Sometimes:
- partial strings
- corrupted fragments
- or tiny leaks

are enough to recover the entire secret.

The important part was recognizing patterns instead of blindly assuming everything had to be fully reversed.

Packed binaries may hide information, but they often leak just enough for careful analysis to piece things together.
