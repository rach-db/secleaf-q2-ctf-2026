# almost_there – Writeup

## Challenge Description

> The backup archive seems damaged.  
> But maybe not everything is lost.

---

# Initial Analysis

The challenge provided a ZIP archive:

```txt
backup.zip
```

The first step was identifying the file type:

```bash
file backup.zip
```

Output:

```txt
Zip archive data
```

So the file was still recognized as a ZIP archive.

However, extraction failed:

```bash
unzip backup.zip
```

Output:

```txt
bad zipfile offset (local header sig): 0
```

This indicated that the ZIP structure itself had been corrupted.

---

# Attempting Recovery

I first attempted automated recovery using common ZIP repair methods.

Using `7z`:

```bash
7z x backup.zip
```

Result:

```txt
Cannot open the file as [zip] archive
```

Then using ZIP repair:

```bash
zip -FF backup.zip --out fixed.zip
```

Output:

```txt
no local entry: flag.txt
```

This revealed something important:

- the archive metadata still referenced `flag.txt`
- but the local ZIP header itself was damaged

This strongly suggested intentional corruption of the ZIP structure.

---

# Inspecting the Archive Header

I inspected the raw bytes using:

```bash
xxd backup.zip
```

A normal ZIP archive begins with the magic bytes:

```txt
50 4B 03 04
```

which corresponds to:

```txt
PK..
```

However, the archive actually began with:

```txt
00 00 03 04
```

The first two bytes:

```txt
50 4B
```

had been replaced with:

```txt
00 00
```

Everything else in the archive remained intact.

---

# Repairing the ZIP Signature

The corrupted ZIP signature was repaired manually by restoring:

```txt
PK\x03\x04
```

Once the header was corrected, the archive structure became valid again.

---

# Recovering the Flag

Even before extraction, the flag was visible directly inside the archive data during hex inspection.

The following hexadecimal sequence appeared:

```txt
5365 634c 6561 667b 7265 7061 6972 ...
```

Converting the hexadecimal ASCII sequence revealed:

```txt
SecLeaf{repair_the_archive}
```
---

# Tools Used

- `file`
- `unzip`
- `7z`
- `xxd`
---

# Key Takeaway

This challenge demonstrated an important forensic concept:

> Small structural corruption can prevent normal parsing even when the actual data still exists.

The intended solve path was:
1. recognize the damaged ZIP signature
2. inspect the archive at the byte level
3. manually repair the header
4. recover the embedded data
