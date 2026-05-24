# force-push-wont-save-you – Writeup

## Challenge Description

> The developers tried to clean their Git history before publishing the repository.
>
> Unfortunately for them, Git remembers more than they expected.

---

# Initial Analysis

The challenge provided a Git repository.

The title:

```txt
force-push-wont-save-you
```

immediately suggested:
- deleted Git history
- hidden commits
- dangling objects
- stash recovery
- or leftover repository metadata

The phrase “force push” strongly implied that important data may still exist somewhere inside `.git`.

---

# Enumerating the Repository

I began by inspecting the commit history:

```bash
git log --all
```

Several suspicious commits and fake-looking flags appeared.

Additional investigation included:

```bash
git reflog
git stash list
git fsck --lost-found
```

This revealed multiple misleading artifacts such as:

```txt
SecLeaf{still_fake}
SecLeaf{not_the_real_flag}
SecLeaf{fake_dangling_flag}
```

---

# Recognizing the Misdirection

At this point, it became clear that the challenge intentionally contained multiple fake flags.

This was an important clue.

Instead of trusting:
- commit history
- dangling blobs
- stash entries

I shifted focus toward:
- Git internals
- hidden metadata
- less commonly inspected files

The challenge title hinted that:

> history itself might be the trap.

---

# Inspecting the `.git` Directory Directly

Rather than relying only on Git commands, I searched the raw `.git` directory directly:

```bash
grep -R "SecLeaf" .git/
```

This revealed the real flag hidden inside:

```txt
.git/info/exclude
```

The output contained:

```txt
SecLeaf{history_was_the_trap}
```

---

# Why This Worked

The file:

```txt
.git/info/exclude
```

is:
- local-only Git metadata
- not committed into repository history
- rarely inspected during normal Git analysis

Most players focus only on:
- commits
- reflog
- stash
- dangling blobs

The challenge exploited that tunnel vision perfectly.
---

# Tools Used

- `git log`
- `git reflog`
- `git fsck`
- `grep`

---

# Key Takeaway

This challenge demonstrated an important forensic principle:

> Sometimes the metadata surrounding the history is more important than the history itself.

The intended lesson was:
- inspect Git internals directly
- recognize deliberate CTF misdirection
