# force-push-wont-save-you – Writeup

## Challenge Description

> The developers tried to clean their Git history before publishing the repository.
>
> Unfortunately for them, Git remembers more than they expected.

---

# First Impressions

The challenge provided a Git repository.

The title:

```txt
force-push-wont-save-you
```

immediately suggested a challenge involving:
- deleted Git history
- hidden commits
- dangling objects
- stash recovery
- or leftover Git metadata.

The phrase “force push” was the biggest clue.

Normally, when developers try to remove sensitive information from a repository, they rewrite history and force push the cleaned version. But in CTFs, that usually means:
> something still survived somewhere inside `.git`.

---

# Exploring the Repository

I started with the obvious Git investigation commands:

```bash
git log --all
```

Then:

```bash
git reflog
git stash list
git fsck --lost-found
```

Very quickly, I started finding suspicious-looking flags like:

```txt
SecLeaf{still_fake}
SecLeaf{not_the_real_flag}
SecLeaf{fake_dangling_flag}
```

At first this looked promising, but after seeing multiple fake flags, it became obvious that the challenge was intentionally trying to mislead players.

---

# Recognizing the Trap

This was the most important realization in the challenge.

One fake flag is normal in CTFs.

But once there are several fake flags spread across:
- commits
- stash entries
- dangling blobs
- recovered objects

…it usually means the challenge author wants you to focus too much on Git history itself.

That’s when I stopped trusting the obvious artifacts and started thinking:

> what if the history itself is the distraction?

The challenge title suddenly made much more sense.

---

# Searching Git Internals Directly

Instead of only using Git commands, I searched the raw `.git` directory directly:

```bash
grep -R "SecLeaf" .git/
```

This immediately revealed something interesting inside:

```txt
.git/info/exclude
```

The output contained:

```txt
SecLeaf{history_was_the_trap}
```

And that turned out to be the real flag.

---

# Why This Was Clever

The file:

```txt
.git/info/exclude
```

is:
- local Git metadata
- not part of commit history
- not pushed normally
- and rarely inspected during standard Git analysis.

Most people naturally focus on:
- commits
- reflogs
- stashes
- dangling objects

The challenge exploited that tunnel vision perfectly.

The real trick was realizing that:
> Git contains more than just commit history.

---

# Tools Used

- `git log`
- `git reflog`
- `git fsck`
- `grep`

---

# What I Learned

This challenge was less about advanced Git recovery and more about mindset.

The biggest lesson was:
- don’t blindly trust the obvious path
- recognize intentional misdirection
- and inspect the surrounding metadata, not just history itself.

Ironically, the challenge title was telling the truth the entire time:

> the history really was the trap.
