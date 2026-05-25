# almost_there – Writeup

## Challenge Description

> The backup archive seems damaged.  
> But maybe not everything is lost.

---

# First Impressions

The challenge provided a ZIP archive named:

```txt
backup.zip
```

At first glance, it looked like a normal archive, so I started with the usual checks.

I identified the file type using:

```bash
file backup.zip
```

Output:

```txt
Zip archive data
```

So the file was still being recognized as a ZIP archive.

That usually means:
- the structure is partially intact
- or only a small part of the archive is corrupted.

---

# Trying to Extract the Archive

The obvious next step was attempting extraction:

```bash
unzip backup.zip
```

But extraction failed with:

```txt
bad zipfile offset (local header sig): 0
```

That error strongly suggested the ZIP structure itself had been damaged.

At this point, the challenge clearly became a forensic repair problem rather than a normal extraction task.

---

# Attempting Automatic Recovery

Before manually editing anything, I tried standard recovery tools.

First with `7z`:

```bash
7z x backup.zip
```

which failed with:

```txt
Cannot open the file as [zip] archive
```

Then I tried ZIP repair:

```bash
zip -FF backup.zip --out fixed.zip
```

This produced an interesting message:

```txt
no local entry: flag.txt
```

That was important because it confirmed:
- the archive still referenced `flag.txt`
- but some internal ZIP structures were broken.

So the archive wasn’t completely destroyed — only partially corrupted.

---

# Inspecting the Raw Bytes

At this point, I inspected the archive manually using:

```bash
xxd backup.zip
```

A normal ZIP file starts with the magic bytes:

```txt
50 4B 03 04
```

which corresponds to:

```txt
PK..
```

But this archive instead began with:

```txt
00 00 03 04
```

That immediately stood out.

The first two bytes:

```txt
50 4B
```

had simply been replaced with:

```txt
00 00
```

Everything else in the archive still looked valid.

So the corruption was actually very small.

---

# Repairing the ZIP Header

The fix was straightforward:
restore the missing ZIP signature.

Replacing the damaged bytes with:

```txt
PK\x03\x04
```

repaired the archive structure.

This was a nice reminder that sometimes a file looks “broken” simply because a parser can no longer recognize its header correctly.

---

# Recovering the Flag

While inspecting the archive in hex form, I noticed a readable ASCII sequence hidden directly inside the data:

```txt
5365 634c 6561 667b 7265 7061 6972 ...
```

Converting the hexadecimal bytes to ASCII revealed:

```txt
SecLeaf{repair_the_archive}
```

So the flag was actually recoverable even before fully repairing the archive.

---

# Tools Used

- `file`
- `unzip`
- `7z`
- `zip`
- `xxd`

---

# What I Learned

This challenge was a good example of how small structural corruption can completely break file parsing.

The archive wasn’t truly destroyed — it only had a damaged signature.

The important part was:
- recognizing ZIP magic bytes
- inspecting the file at the byte level
- and not assuming automated tools will always recover corrupted data.

Sometimes a tiny manual fix is all that’s needed to recover everything.
