# Forgotten Snapshot - Writeup

## Challenge Description

> The past is never truly deleted.  
> Sometimes old snapshots still remember what was lost.

---

## Initial Analysis

The challenge provided a disk / filesystem snapshot image.

The title **"Forgotten Snapshot"** strongly suggested:

- old filesystem state
- deleted files
- VM/container snapshot
- hidden historical data
- recoverable artifacts from previous versions

This immediately pointed toward filesystem forensics and recovery techniques.

---

## Initial Enumeration

First, the file type was identified:

```bash
file <challenge_file>
```

Then basic string extraction was performed:

```bash
strings <challenge_file> | less
```

Nothing immediately revealed the flag, so deeper forensic analysis was required.

---

## Filesystem Investigation

The snapshot was mounted / extracted for inspection.

Common forensic enumeration steps included:

```bash
binwalk <challenge_file>
```

and:

```bash
foremost <challenge_file>
```

as well as checking for recoverable deleted data.

The challenge hinted that older data still existed inside the snapshot even though it was no longer visible in the active filesystem.

---

## Recovering Historical Data

The important realization was:

> snapshots preserve older filesystem states

Even if a file is deleted later, older snapshots may still contain:

- previous versions
- deleted content
- hidden metadata
- cached artifacts

The filesystem contents were explored carefully until the hidden artifact containing the flag was recovered.

---

## Key Insight

The challenge was not about cracking encryption or reversing binaries.

It was about understanding:
- snapshot behavior
- forensic recovery

Old snapshots often preserve data that users believe was deleted permanently.

---

## Tools Used

- `file`
- `strings`
- `binwalk`
- `foremost`
- Linux filesystem utilities

---

## Takeaway

This challenge demonstrates an important forensic concept:

> Deleted does not always mean gone.

Snapshots, backups, and filesystem history can preserve sensitive data long after users think it has been removed.
