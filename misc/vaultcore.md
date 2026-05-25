# vaultcore – Writeup

## Challenge Description

> A secure vault system was recovered from an abandoned internal server.
>
> The developers claimed the protection mechanism was impossible to bypass.
>
> Can you uncover what lies inside the vault?

---

# First Impressions

The challenge provided a binary named:

```txt
vaultcore
```

From the name alone, it already sounded like a classic reverse engineering challenge involving:
- password checks
- hidden validation logic
- or some kind of internal “vault” mechanism.

So instead of trying random inputs immediately, I started by inspecting how the program was built.

---

# Looking at the Binary

The first step was identifying the file type:

```bash
file vaultcore
```

Then I extracted readable strings from the binary:

```bash
strings vaultcore
```

This turned out to be the most useful step early on.

The output revealed:
- validation-related messages
- suspicious-looking text
- hidden function names
- and fragments that looked important.

That immediately suggested the binary wasn’t heavily protected and was likely leaking information directly through embedded strings.

---

# Understanding the Logic

After that, I started inspecting the binary more carefully using tools like:

```bash
objdump
```

and general static analysis techniques.

The goal was to understand:
- how user input was handled
- where comparisons happened
- and whether any hidden decoding logic existed internally.

The binary seemed to follow a very typical pattern:
1. take user input
2. transform or verify it
3. compare it against an internal expected value

At this point, the challenge became less about brute force and more about understanding the program flow.

---

# The Important Realization

The biggest realization during analysis was that the binary wasn’t using any serious cryptography or advanced protection.

Most of the “security” came from:
- obfuscation
- hidden constants
- misleading function names
- and making the binary look more complicated than it actually was.

Once the validation logic was traced properly, reconstructing the hidden value became much easier.

---

# Recovering the Flag

By following the verification routine and analyzing the internal logic, the hidden secret inside the vault was recovered successfully.

---

# Tools Used

- `file`
- `strings`
- `objdump`
- Linux terminal utilities

---

# What I Learned

This challenge reinforced a really important reverse engineering lesson:

> always start simple.

It’s easy to assume a binary is doing something complex, but many challenges leak important information through:
- embedded strings
- debug text
- obvious comparisons
- or poorly hidden constants.

In this case, simple static analysis gave away most of what was needed before any deep reversing was necessary.
