# Wander & Bloom — Claude Instructions

## Project overview
Wander & Bloom is a free photography garden map of the Netherlands. A single-page app listing 100+ gardens, filterable by category and province, with an interactive Leaflet map. Made by Marianne Eggink.

- **Live URL:** https://mariannesphotos.github.io/wanderbloom/
- **Hosting:** GitHub Pages (static — no server, no build step)
- **Stack:** Vanilla HTML, CSS, JavaScript. No frameworks, no npm.

---

## File structure

| File | Purpose |
|------|---------|
| `index.html` | All HTML markup, cookie bar, GA script |
| `styles.css` | All styling |
| `script.js` | All data (GARDENS, GARDEN_PHOTOS) and all logic |
| `sitemap.xml` | SEO sitemap — must be kept in sync with gardens |
| `robots.txt` | Points Google to sitemap |
| `privacy.html` | Privacy policy page — linked from footer |

---

## Design system

**Colors**
- `--sage: #7a9e7e`
- `--sage-light: #a8c5aa`
- `--sage-mid: #5e8462`
- `--sage-dark: #4a6b4e`
- `--cream: #faf6ee`
- `--warm-white: #e2c9c0`
- `--terracotta: #c4785a`
- `--charcoal: #2d2d2a`
- `--mid: #6b6b67`
- `--border: #ddd8cc`
- Hero/panel dark green: `#1e3322`

**Fonts**
- Body: `DM Sans` (weights 300, 400, 500, 600, 700)
- Headings: `Playfair Display` (serif, italic for emphasis)

**Breakpoints**
- `max-width: 780px` — tablet/mobile layout
- `max-width: 480px` — small mobile

---

## Garden data structure

All garden data lives in `script.js`. There are two main objects:

### GARDENS array
Each garden object:
```javascript
{
  name: "Garden Name",           // string, required — must be unique
  lat: 52.123,                   // number, required
  lng: 4.456,                    // number, required
  category: "Botanical Gardens", // string, required — see categories below
  province: "Utrecht",           // string, required — see provinces below
  city: "Amsterdam",             // string, optional but include if known
  featured: false,               // boolean, required
  has_reel: false,               // boolean, required — true if reel_link is set
  reel_link: null,               // string | null — Instagram reel/post URL
  reel_label: "Instagram Post",  // string, optional — defaults to "Instagram Reel"
  visitor_link: "https://...",   // string, required — Google Maps URL
  preview: "",                   // string — short preview (leave empty if unknown)
  description: "",               // string — full description (leave empty if unknown)
  featured_img: "https://i.imgur.com/xxx.jpeg", // optional — used when no GARDEN_PHOTOS entry
}
```

**Important:** Never invent or fill in `preview` or `description` text. Leave them as empty strings `""` if not provided by Marianne.

### GARDEN_PHOTOS object
```javascript
const GARDEN_PHOTOS = {
  "Exact Garden Name": [
    "https://i.imgur.com/xxx.jpeg",
    "https://i.imgur.com/yyy.jpeg",
    // ...
  ],
}
```
- Key must exactly match `name` in GARDENS
- All photos are hosted on Imgur
- First photo in array is used as the featured image
- No entry needed if garden has no photos

---

## Categories (12 total)
These are the only valid category values. Each has associated icon, colors, and description already defined in script.js.

1. Art & Sculpture Gardens
2. Botanical Gardens
3. Butterfly Gardens
4. Historical Gardens
5. Native Plant Gardens
6. Private Gardens
7. Show Gardens
8. Tea Gardens
9. Tree Gardens
10. Tulip Gardens
11. World Gardens
12. Zoo Gardens

**Adding a new category** requires updating all of these in script.js:
- `ICONS` — Imgur icon URL
- `CAT_COLORS` — light hex color
- `CAT_DARK_COLORS` — dark hex color
- `CAT_DESC` — description text
- `ALL_CATS` — canonical array

---

## Provinces (12 total)
```
Drenthe, Flevoland, Friesland, Gelderland, Groningen,
Limburg, Noord-Brabant, Noord-Holland, Overijssel,
Utrecht, Zeeland, Zuid-Holland
```

---

## Checklist: adding a new garden

- [ ] Add object to `GARDENS` array in `script.js`
- [ ] Add entry to `GARDEN_PHOTOS` in `script.js` (if photos provided)
- [ ] Add `<url>` entry to `sitemap.xml` using the correct slug
- [ ] Commit and push

---

## Checklist: updating an existing garden

- [ ] Update the garden object in `GARDENS`
- [ ] Update `GARDEN_PHOTOS` if photos changed (key = exact garden name)
- [ ] If the garden `name` changed: update the `GARDEN_PHOTOS` key, the sitemap slug, and verify the deep link still works
- [ ] Commit and push

---

## Slug generation
Used for URL deep links (`?garden=slug`) and sitemap entries.

```javascript
function gardenSlug(name) {
  return name.toLowerCase().replace(/[^a-z0-9]+/g, "-").replace(/^-|-$/g, "");
}
```

Examples:
- `"Hortus Botanicus Amsterdam"` → `"hortus-botanicus-amsterdam"`
- `"Tuin Château St. Gerlach"` → `"tuin-ch-teau-st-gerlach"`
- `"A.Vogel Tuinen"` → `"a-vogel-tuinen"`

Sitemap URL format: `https://mariannesphotos.github.io/wanderbloom/?garden={slug}`

---

## Featured gardens
The `FEATURED_ORDER` array at the top of `script.js` controls which gardens appear in the featured carousel and in what order. To feature a garden, set `featured: true` in its GARDENS entry and add its name to `FEATURED_ORDER`.

---

## SEO
- `sitemap.xml` must stay in sync with GARDENS — add a new `<url>` entry for every new garden
- `robots.txt` points to the sitemap — do not modify
- `document.title` updates dynamically when a garden sheet opens (already implemented)
- The Google site verification file `google086ba0119ec8d4be.html` must not be deleted

---

## Behaviour rules
- **Always push after committing** — no need to ask
- **Never invent garden descriptions or preview text** — leave as `""` if not provided
- **No filler text** for empty fields
- **No npm, no build tools, no frameworks** — keep it vanilla
- When adding a garden, use the exact data Marianne provides — do not guess coordinates, categories, or links
