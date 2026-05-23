# Can_you_Find_Cafe – Writeup

## Challenge Description

> A single image holds all the clues you need. Study the surroundings carefully and identify the exact location where it was taken.

### Flag Format

```txt
SecLeaf{Name_of_the_place+Location_name}
```

---

## Initial Analysis

The challenge provided a single image named:

```txt
whereami.jpeg
```

The image appeared to show:

- a heritage-style building
- curved colonial-era balcony architecture
- dense urban Indian surroundings
- older commercial street layout

From the architectural style and surroundings, the location looked like an older Indian metropolitan district, possibly:

- Pune
- Mumbai
- Kolkata

At this stage, no exact location was confirmed.

---

## Reverse Image Search

To identify the location, I used:

- Yandex Reverse Image Search
- Google Lens

### Yandex Results

Yandex produced the first strong lead related to:

```txt
Ganesh Gruha – Art Deco building in Pune
```

Although this was not the final answer, it established:

- Pune as a likely city
- a strong architectural similarity with the challenge image

---

## Contextual Correlation

Next, I used Google Lens for additional contextual matches.

Google Lens returned results associated with:

- FC Road
- Deccan Gymkhana
- cafés in Pune

This suggested that:

- the building itself might be a well-known café
- the image was likely taken near FC Road / Deccan Gymkhana

At this point, Pune became a high-confidence match.

---

## Final Confirmation

The decisive confirmation came from a Facebook / Urban Sketchers reference image.

The post showed:

- the exact same façade
- matching balcony structure
- identical architectural details

The post explicitly mentioned:

```txt
Cafe GoodLuck
F.C. Road
Deccan Gymkhana
Pune
```

This confirmed:

- the café name
- the surrounding locality
- the exact visual match

---

## Flag Construction

The remaining challenge was determining the correct flag formatting.

Several formatting combinations were considered, including:

```txt
Cafe_GoodLuck+FC_Road
Cafe_GoodLuck+Pune
Cafe_GoodLuck+Deccan_Gymkhana
```

The correct flag was:

```txt
SecLeaf{Cafe_GoodLuck+Deccan_Gymkhana}
```

---

## Key Takeaways

This challenge focused heavily on:

- reverse image search techniques
- architectural recognition
- contextual OSINT correlation
- location narrowing through external references

