# Working on this site

Website for Abide House Columbus, a faith-based hair salon and barbershop in
German Village, Columbus OH. Two businesses under one roof: **The Abide House
Salon** and **Crowned With Glory Barbershop**.

Plain HTML and CSS. No build step, no framework, no dependencies. Everything is
in two files with inline `<style>` blocks.

**Deploying:** commit to `main` and push. Netlify publishes automatically in
about 30 seconds. There's nothing to build or run. If a change goes wrong, it
can be rolled back instantly from Netlify's Deploys tab, or by reverting the
commit.

**Live at:** https://abidehousecolumbus.com

## Files

| Path | What it is |
| --- | --- |
| `index.html` | Homepage: hero, Our Artists, Our Home galleries, Visit |
| `about.html` | The story, three co-founder cards, two scripture passages |
| `Pictures/web/` | Every image the site loads |

## Images: read this before adding any

**Only `Pictures/web/` is served.** It holds web-sized copies: resized,
compressed, and cropped for where they appear.

The full-resolution camera originals sit in `Pictures/` and are **gitignored on
purpose**. They total ~88MB. The site never loads them, and Git stores every
version of a binary in full, so committing them would bloat the repo
permanently. They live in the owner's OneDrive folder as the source for
re-cropping.

Originally the homepage loaded 77.5MB of images. It's now under 3MB. **Do not
commit a camera original into `Pictures/web/`** — resize it first. Rough
targets: hero ~2000px wide, gallery photos ~1400px, carousel photos ~900px,
portraits 800x1200.

### The logos are matted PNGs, not the files the owner sent

`abide-house-logo.png`, `crowned-with-glory-logo.png`, `favicon.png`, and
`apple-touch-icon.png` all came from artwork with a solid background baked in.
The originals were white and greige rectangles. Each was converted to a
transparent PNG by deriving alpha from luminance, so the ink now sits directly
on the ivory and blush backgrounds with no box around it.

If one of these is ever re-exported, matte it the same way. Dropping the
original JPEG in will put a visible rectangle back on the page.

The nav logo and the nav bar height are a pair. The bar is 88px so the
"Abide House / Salon & Barbershop" lines under the arch stay readable at 76px.
Below roughly 72px they turn to mush. Shrink both together or neither.

`favicon.png` is the A/H monogram alone, cropped out of the arch, because the
full lockup is unreadable at 16px. `apple-touch-icon.png` is the whole arch,
which reads fine at home screen sizes.

### Some images are cropped differently per device

These pairings are deliberate. Changing one side without the other will break
the layout.

**Hero.** Desktop uses `Hero.jpg` (wide landscape). Phones at 700px and below
swap to `Hero-mobile.jpg`, a portrait crop that keeps both the sign and the
front door in frame. The mobile rule sets `min-height:min(128vw,760px)` — that
`128vw` is the crop's aspect ratio, which is why almost nothing gets cut. If
the mobile hero image is ever recropped, recalculate that number.

**Shelby's card.** Her background is set in CSS (`.photo.shelby`), *not* inline,
specifically so a media query can override it. Desktop gets
`shelby-card.jpg`, a tight head-to-waist crop, because the desktop card slot is
narrow and tall and a full-body shot renders her face at ~25px. Below 560px it
switches to `shelby-full.jpg` at `center 75%`, which shows the clippers and
tools at her waist that she wants visible. Putting an inline `style` back on
that element would break the swap.

**Portraits** use `background-position:center 20%`. At `center top` the mobile
cards clipped chins.

**Gallery mosaic (under 600px).** The first photo in each group spans both
columns. The barbershop's *last* photo also spans, because it has 4 photos and
an even count would otherwise leave one sitting alone. The salon has 5 and
doesn't need it. **If photo counts change, revisit that rule** — it's keyed to
`:first-child` / `:last-child`.

## Copy conventions

- **No em dashes.** The owner has asked for these removed repeatedly. Use a
  period or a comma.
- **"Our Artists"**, never "Our Team" or "Stylists". It has to cover both
  hairstylists and barbers.
- **"Crowned With Glory Barbershop"** — *Crowned*, not *Crown*. This has been
  corrected once already.
- Section names: "Our Artists", "Our Home", "About", "Visit".

## Never fabricate content

This is a real business. An invented "5.0 based on GlossGenius reviews" rating
was live at one point and had to be removed. **Do not add star ratings,
reviews, testimonials, hours, or credentials that weren't supplied.** If
something is unknown, leave it out rather than inventing a placeholder — and
don't ship visible placeholder text like "add here", which real customers see.

Booking links are per-artist and point to their own external systems
(GlossGenius, Square). Phone numbers are real `tel:` links.

## Things that look odd but are intentional

- The **faith band carousel** clones its slides and appends them, so scrolling
  past the last photo runs seamlessly into the copies before snapping back.
  Removing the clone step reintroduces a blank gap at the end.
- The **map iframe** starts at `opacity:0` and fades in via its `onload`
  handler, with `preconnect` hints in `<head>`. This was to fix a slow, jarring
  pop-in on mobile.
- **Team cards are `<button>` elements** with an explicit `color`. Without it,
  mobile Safari renders the team names in default link blue.

## After making changes

Check that every image reference still resolves — a typo in a filename produces
a silently broken image rather than an error:

```bash
for h in index.html about.html; do
  grep -oP "Pictures/[^')\"]+" "$h" | sort -u | while IFS= read -r r; do
    [ -f "$r" ] || echo "MISSING [$h]: $r"
  done
done
```

Both files carry Open Graph tags with **absolute** URLs pointing at
`abidehousecolumbus.com`, plus a 1200x630 `social-preview.jpg`. If the domain
ever changes, those need updating or link previews break.
