# Can_you_Find_Cafe – Writeup

## Challenge Description

> A single image holds all the clues you need. Study the surroundings carefully and identify the exact location where it was taken.

### Flag Format

```txt
SecLeaf{Name_of_the_place+Location_name}
```

---

# First Impressions

The challenge only provided a single image:

```txt
whereami.jpeg
```

At first glance, the image looked like a normal street photograph, but a few details immediately stood out:
- old heritage-style architecture
- curved balconies
- dense urban surroundings
- older commercial building design

The overall vibe felt very “old city India.”

My first guesses were places like:
- Pune
- Mumbai
- Kolkata

because those cities have a lot of colonial-era and Art Deco style buildings.

At this stage though, there wasn’t enough information to confidently identify the exact location.

---

# Reverse Image Search

Since the challenge was clearly OSINT-focused, I started with reverse image searching.

I used:
- Yandex Reverse Image Search
- Google Lens

Yandex gave the first useful lead related to:

```txt
Ganesh Gruha – Art Deco building in Pune
```

This wasn’t the exact answer, but it was important because:
- the architecture looked extremely similar
- it strongly suggested the city was Pune

So now the search became much narrower.

---

# Narrowing the Location

Next, I used Google Lens to look for more contextual matches.

This started returning results connected to:
- FC Road
- Deccan Gymkhana
- cafés in Pune

That was the first moment where things started clicking together.

The building in the challenge image didn’t just look like a random structure anymore — it looked like a famous café or landmark building in Pune.

---

# Finding the Exact Match

The final confirmation came from a Facebook / Urban Sketchers reference image.

The image showed:
- the exact same façade
- matching curved balconies
- identical architectural details

Most importantly, the post explicitly mentioned:

```txt
Cafe GoodLuck
F.C. Road
Deccan Gymkhana
Pune
```

At that point, the match became very high confidence.

---

# Constructing the Flag

The only thing left was figuring out the exact flag format.

I tried a few possible combinations like:

```txt
Cafe_GoodLuck+FC_Road
Cafe_GoodLuck+Pune
Cafe_GoodLuck+Deccan_Gymkhana
```

The correct flag turned out to be:

```txt
SecLeaf{Cafe_GoodLuck+Deccan_Gymkhana}
```

---

# What I Learned

This challenge was a really good example of practical OSINT.

The important part wasn’t just reverse searching the image — it was:
- correlating architecture
- narrowing locations step-by-step
- validating matches from multiple sources
- and paying attention to contextual clues

One thing I liked about this challenge is that it felt very realistic.

You rarely get the exact answer immediately from reverse image search. Usually you get partial leads, and then you build confidence gradually until the pieces fit together.
