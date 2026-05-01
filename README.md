# CF22 Map Config Data

Data repository for the **[Unofficial CF22 Interactive Map](https://github.com/mentegago/cf22-map)** — a fan-made interactive map for Comifuro 22.

Served via GitHub Pages at `https://cf22-config.nnt.gg`.

## Overview

This repo holds the processed catalog data consumed by the map app. It is synchronized automatically to [Comifuro Web Catalog](https://catalog.comifuro.net) several times a day.

## Files

### `data/creator-data.json`

The main catalog. Contains all creator/circle entries with normalized booth locations, fandoms, and links.

```jsonc
{
  "version": 36,
  "creators": [
    {
      "id": 1234,
      "user_id": "uuid-...",
      "name": "Circle Name",
      "booths": ["AA-01", "AA-02"],
      "day": "BOTH",              // "BOTH" | "SAT" | "SUN"
      "urls": [
        { "title": "Instagram", "url": "https://instagram.com/..." }
      ],
      "raw_fandoms": ["genshin impact", "hsr"],
      "fandoms": ["Genshin Impact", "Honkai Star Rail"],
      "works_type": ["Artbook", "Goods"],
      "sampleworks_images": ["https://..."],
      "circle_cut": "https://...",
      "circle_code": "AA-01/AA-02"
    }
  ]
}
```

**`creators` object fields:**

| Field | Type | Description |
|-------|------|-------------|
| `id` | `number` | Numeric ID from the Comifuro Web Catalog |
| `user_id` | `string` | UUID from the Comifuro Web Catalog |
| `name` | `string` | Circle/creator display name |
| `booths` | `string[]` | List of booth IDs this circle occupies (e.g. `["AA-01", "AA-02"]`) |
| `day` | `string` | Which event day(s) the circle is present: `BOTH`, `SAT`, or `SUN` |
| `urls` | `object[]` | Social/portfolio links; each entry has `title` (platform name) and `url` |
| `raw_fandoms` | `string[]` | Original fandom strings as entered by the creator (may be inconsistent) |
| `fandoms` | `string[]` | Normalized canonical fandom names (smart fandom grouping) |
| `works_type` | `string[]` | Types of merchandise/works being sold: `Artbook`, `Comic`, `Goods`, `Novel`, `Game`, `Music`, `Photobook General`, `Photobook Cosplay`, `Handmade Crafts`, `Magazine`, `Commission` |
| `sampleworks_images` | `string[]` | URLs to sample work images uploaded by the creator |
| `circle_cut` | `string` | URL to the circle's cover/promotional image |
| `circle_code` | `string` | Raw booth code string as it appears in the Comifuro Web Catalog (e.g. `"AA-01/AA-02"`) |

---

### `data/fandom-groups.json`

Canonical fandom name → known variant spellings. Used to normalize the messy free-text fandom entries that creators submit.

```jsonc
[
  {
    "canonical": "Honkai Star Rail",
    "variants": ["Honkai Star Rail", "hsr", "HSR", "Honkai Starrail", "..."]
  }
]
```

---

### `data/popular-fandoms.json`

Fandom popularity ranking — how many circles are participating per fandom, sorted descending.

```jsonc
[
  { "fandom": "Original", "count": 490 },
  { "fandom": "Honkai Star Rail", "count": 353 },
  { "fandom": "Genshin Impact", "count": 339 }
]
```

---

### `version.json`

Versioning metadata used by the [map app](https://github.com/mentegago/cf22-map).

```jsonc
{
  "current_version": 10,
  "release_notes": "...",
  "creator_data_version": 36
}
```

| Field | Type | Description |
|-------|------|-------------|
| `current_version` | `number` | Increments when a change requires users to hard-refresh the app. The app compares this against the cached value and shows a prompt if they differ. |
| `release_notes` | `string` | Message displayed to the user in the refresh prompt. |
| `creator_data_version` | `number` | Increments with every catalog sync. Mirrors the `version` field inside `creator-data.json`. |

When a newer `creator_data_version` is available, the app downloads `creator-data.json` in the background, saves it to local storage, and updates the UI.

## Usage

Fetch directly from the GitHub Pages URL:

```
https://cf22-config.nnt.gg/data/creator-data.json
https://cf22-config.nnt.gg/data/fandom-groups.json
https://cf22-config.nnt.gg/data/popular-fandoms.json
https://cf22-config.nnt.gg/version.json
```

GitHub Pages [serves all content with `Access-Control-Allow-Origin: *`](https://github.com/orgs/community/discussions/22399), so these endpoints can be called directly from any web frontend without a proxy.

Or clone and read locally for offline use.

## Data Source

Raw catalog data is sourced from the official Comifuro Web Catalog at [catalog.comifuro.net](https://catalog.comifuro.net). All catalog content, including circle information and images, is copyright of Comifuro and/or their respective creators.
