# Forgotten Snapshot – Writeup

## Challenge Description

> The past is never truly deleted.  
> Sometimes old snapshots still remember what was lost.

---

# First Impressions

The challenge provided a filesystem / snapshot image.

The title:

```txt
Forgotten Snapshot

```
![Snapshot Analysis](images/snapshot.jpg)

immediately made me think about:
- deleted files
- old filesystem states
- VM or container snapshots
- recoverable historical data

So right from the beginning, this felt more like a forensic recovery challenge than a traditional cracking challenge.

The key idea was probably:
> something important existed in an older state of the filesystem and was never completely removed.

---

# Initial Enumeration

I started with the usual basic checks:

```bash
file <challenge_file>
```

Then extracted readable strings:

```bash
strings <challenge_file> | less
```

Nothing obvious appeared immediately, which suggested that the flag wasn’t simply embedded as plaintext.

At that point, deeper forensic inspection was necessary.

---

# Investigating the Snapshot

Since the challenge involved a snapshot image, the next step was exploring the filesystem contents more carefully.

I used tools like:

```bash
binwalk <challenge_file>
```

and:

```bash
foremost <challenge_file>
```

to inspect embedded structures and recover possible deleted artifacts.

The challenge description strongly hinted that:
- old data still existed somewhere inside the snapshot
- even if it was no longer visible in the “current” filesystem state.

---

# The Important Realization

The key realization during the challenge was:

> snapshots preserve history.

Even when users delete files later, older snapshots may still contain:
- previous file versions
- deleted documents
- cached content
- metadata remnants
- hidden historical artifacts

So instead of treating the image like a normal file, I approached it like a preserved timeline of filesystem states.

---

# Recovering the Hidden Data

By carefully exploring the recovered filesystem data and extracted artifacts, the hidden content containing the flag was eventually recovered.

The challenge wasn’t about:
- brute force
- cryptography
- or reversing binaries

It was about understanding how persistent storage and snapshots work in real forensic situations.

---

# Tools Used

- `file`
- `strings`
- `binwalk`
- `foremost`
- Linux filesystem utilities

---

# What I Learned

This challenge was a really good reminder that:

> deleted does not always mean gone.

Snapshots, backups, and filesystem history can preserve sensitive data long after users believe it has been removed permanently.

From a forensic perspective, historical data can often be more valuable than the active filesystem itself.0
