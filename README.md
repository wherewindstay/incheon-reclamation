**English** | [한국어](README.ko.md)

# Incheon, a City Built on Reclaimed Sea (1910–2025)

[![Incheon reclamation map — shorelines from 1910 to 2025](preview.png)](https://wherewindstay.github.io/incheon-reclamation/)

> ### **[▶ Open the map — wherewindstay.github.io/incheon-reclamation](https://wherewindstay.github.io/incheon-reclamation/)**
>
> *The image above is static. GitHub does not run interactive content inside a README, so follow the link to move through time and click districts.*

An interactive map that stacks Incheon's shoreline like tree rings, showing when and where land was claimed from the sea.

Within this extent, Incheon grew from **766 km² to 1,020 km²**. **19.0%** of today's land (190.3 km²) appeared after 1985, and **13.5%** of the population — about 414,000 people — lives on it.

| District | Share of area reclaimed | Share of population on reclaimed land |
|---|---:|---:|
| Yeonsu-gu | 73.2% | 53.9% |
| Jung-gu | 38.6% | 11.2% |
| Dong-gu | 24.5% | 1.5% |
| Seo-gu | 23.5% | 14.3% |
| Namdong-gu | 17.9% | 11.1% |
| Ganghwa-gun | 9.9% | 4.5% |
| Michuhol-gu | 8.4% | 2.3% |
| Ongjin-gun | 7.5% | 3.4% |
| Bupyeong-gu | 1.3% | 1.7% |
| Gyeyang-gu | 0.9% | 0.6% |
| **Incheon total** | **19.0%** | **13.5%** |

## Data

| Period | Source | Method |
|---|---|---|
| 1910–1955 | National Institute of Korean History, [historical-geographic data](https://hgis.history.go.kr/) | Administrative polygons of each era, dissolved into a land silhouette |
| 1985–2025 (nine snapshots, 5-year intervals) | USGS Landsat 5/7/8/9 Collection 2 Level-2, via Google Earth Engine | Water and land separated with the MNDWI index, vectorized at 30 m |
| District boundaries | Statistics Korea SGIS, 2025 administrative divisions | — |
| Population | Statistics Korea SGIS 100 m grid, 2024 | Cells assigned by whether their centroid falls on reclaimed land |

**Reclaimed land is defined as** 2025 land minus 1985 land.

## How to read it, and what it cannot tell you

- **MNDWI depends on the tide at capture time.** Where tidal flats are wide — west of Yeongjong, south of Ganghwa — shorelines run generous. This is also why area dips slightly from 1990→1995 and 2020→2025.
- **Reclamation before 1985 is invisible here.** Because reclaimed land is defined against the 1985 baseline, places filled earlier (such as the Namdong Industrial Complex) already appear as land in 1985. Cumulative reclamation is larger than these figures suggest.
- **Baengnyeong and Daecheong islands are missing.** The analysis extent stops at 125.65°E.
- **Ongjin-gun's population may be underestimated.** Only three of five SGIS grid zones were available, so some island populations are absent. Area figures are unaffected.
- **1910, 1914 and 1955 are merged into one layer.** Their outlines differ by less than 0.2 km², within measurement error, and the differences come from inland boundary changes rather than the coast.
- **Historical boundaries are clipped to the 1985 satellite extent,** because administrative polygons include tidal flats and run wider than actual land.
- Total land comes to 1,001 km² against an official figure of about 1,065 km² — a 6% gap that reflects where Landsat classification cuts the tidal-flat edge.

## Using the map

- The slider moves through time; layers accumulate up to the selected year
- Click a year in the legend to toggle that layer
- Click a district for its reclamation share, or click open water for the whole city
- Scroll to zoom, drag to pan, double-click to reset
- The **한 · EN** button switches languages

## Technical notes

A single HTML file with no external dependencies. The map is drawn directly in SVG without a mapping library, and all coordinates are inlined, so it works offline and embeds cleanly (for example, in a Notion page).

## Credits

Data collection and the Google Earth Engine notebook by **Sohyun Park**. Data pipeline and visualization built with the help of **Anthropic Claude**. All figures were computed from source data, and validation points — Songdo, Incheon Airport, the Namdong Industrial Complex, and the old city center — were checked against documented reclamation history.

## License

Code released under the MIT License. Source data remains subject to the terms of each provider.
