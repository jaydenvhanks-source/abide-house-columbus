# Abide House Columbus

Website for Abide House Columbus — a faith-based hair salon and barbershop in
German Village, Columbus, OH. Home to The Abide House Salon and Crowned With
Glory Barbershop.

Live site: deployed on Netlify.

## Files

| File | What it is |
| --- | --- |
| `index.html` | Homepage — hero, Our Artists, Our Home galleries, Visit |
| `about.html` | About page — the story, the three co-founders, scripture |
| `Pictures/web/` | Every image the site actually loads |

It's plain HTML and CSS. There's no build step — edit a file, commit, and
Netlify publishes it.

## About the photos

`Pictures/web/` holds web-sized copies: resized, compressed, and cropped for
where they appear. That's the only image folder the site references.

The full-resolution camera originals stay in `Pictures/` locally and are
deliberately **not** committed (see `.gitignore`). They total roughly 88MB and
the site never loads them. Keep them in OneDrive — they're the source if a
photo ever needs re-cropping.

Some images are cropped differently per device. Shelby's card, for example,
uses a tighter head-to-waist crop on desktop where the card is narrow and tall,
and the full-body shot on phones where the card is wide enough to carry it.
The hero does the same: a wide shot on desktop, a portrait crop on phones so
the building and the sign both stay in frame.
