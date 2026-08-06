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
- **Inland classification specks are removed.** The MNDWI classification flickers slightly between years, leaving inland patches (in Bupyeong, Gyeyang and elsewhere) that would read as "land newly created in that period". Reclamation only happens at the coast, so new patches farther than 1.2 km from the shoreline, and fragments under 1 ha, are restored to pre-existing land. Reclaimed-area and population statistics were recomputed on the same basis.
- Total land comes to 1,016 km² against an official figure of about 1,065 km² — a 5% gap that reflects where Landsat classification cuts the tidal-flat edge.

## Trial and Error

The map went through two complete data sources before Landsat. Both failed for reasons that are easy to miss until the data is already in hand, so they are recorded here — anyone reconstructing a Korean coastline is likely to reach for the same two first.

### 1. Administrative boundaries (SGIS, 1975–2025) — they include the sea

The obvious starting point is the official administrative boundary series: eleven snapshots at five-year intervals, high resolution, free. Stack them and the coastline should emerge — but it does not. **Korean administrative boundaries extend over jurisdictional waters.** Songdo, for example, reclaimed from 1994 onward, already sits inside the 1975 boundary of Dongchun-dong, and the Namdong Industrial Complex, filled during the 1980s, sits inside the 1975 boundary of Gojan-dong. The land was not there, but the boundary was.

So a boundary series cannot date reclamation at all. What it shows is when the *administration* reached a place, which may precede the land by decades. We ran an entire version of this map on that misreading before catching it.

The historical polygons (1910–1974) behave differently and are usable — checked against Songdo, Incheon Airport, Cheongna and Namdong, all four correctly read as sea — but they too are wider than actual land, because they include tidal flats. They are clipped to the 1985 satellite extent for that reason.

### 2. Land-cover maps (Ministry of Environment, 1980s–2025) — the sheets stop at 37.5°N

Land-cover classification does give a real shoreline: separate the water class from the land classes and vectorize the edge. Applied to Incheon it produced a plausible series, 330 km² in 1989 rising to 450 km² in 2019, with Songdo and the airport appearing at the right times.

The problem is coverage. **The historical sheets stop at 37.50°N** — the measured extent is 125.75–127.00°E by 37.00–37.50°N. Everything above that line is missing: Cheongna (37.535°N), Geomdan, northern Seo-gu and Gyeyang, and the whole of Ganghwa-gun. The 2025 sheets include the northern tiles; for the four historical periods that band was not available.

### 3. Landsat + MNDWI — trial and error in polygonising

Satellite imagery solved both problems: the extent is whatever you draw, and one index is applied identically to every year. The extraction itself worked on the first try, and the visual check in Earth Engine looked right.

The export did not, and it failed quietly rather than with an error. Three separate causes, all worth knowing:

- **Vectorizing water instead of land.** `reduceToVectors` on the water mask returns sea polygons, which means land exists only as *holes* inside them. Shapefile export dropped the holes: the 4,378 km² sea polygon came back with one ring and zero holes, so the sea covered everything. Sampling the old city centre and Ganghwa returned "sea" for both.
- **A validity filter that discarded the main polygon.** A `if geom.is_valid: ... else: continue` guard looks harmless, but a large polygon holding hundreds of island-holes fails validity easily. The sea body was skipped entirely; the largest surviving feature was a 1.19 km² pond.
- **Shapefile ring-direction conventions.** Shapefile distinguishes outer rings from holes by winding order. Earth Engine's output does not follow it, so readers swap them — pyshp emitted dozens of "this shape consists entirely of holes" warnings.

The fix was to vectorize **land** directly (`water_index.lte(threshold)`), since land is one polygon per island and has no hole problem; to repair rather than discard invalid geometry with `buffer(0)`; and to write **GeoJSON first**, which has no winding convention to violate. We also tried reconstructing polygons from the exported coastline linework, but the rings do not close and only 20–32 km² of land could be recovered per period.

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
