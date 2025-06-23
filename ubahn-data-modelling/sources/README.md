# 🚇 Berlin U-Bahn Data Modeling – Simoun Asmar

## Step 1: Research & Data Modeling  
**Branch:** `ubahn-data-modelling`  
**Focus:** U-Bahn stations and connections in Berlin using cleaned Wikidata and Wikipedia data.

---

## 1.1 Data Source Discovery

### 📍 Source 1: Berlin U-Bahn Stations
- **Name:** Berlin U-Bahn Stations  
- **Source & URL:** Extracted from Wikidata via SPARQL by Clifford Anderson — [GitHub Gist](https://gist.github.com/CliffordAnderson/7fb7473af31f9343f8a55518545480a0)  
- **Type:** Open Data (SPARQL export)  
- **Update Frequency:** Static snapshot (manual)  
- **Data Type:** Static  
- **Key Fields:** `station`, `lat`, `lng`  
- **Notes:** Station names may contain inconsistent formatting (e.g., “U-Bahnhof” prefix)

---

### 🔗 Source 2: U-Bahn Line Connections
- **Name:** Berlin U-Bahn Line Connections  
- **Source & URL:** Scraped from Wikipedia and shared via the same [GitHub Gist](https://gist.github.com/CliffordAnderson/7fb7473af31f9343f8a55518545480a0)  
- **Type:** Open Data / Manual Scraper  
- **Update Frequency:** Static  
- **Data Type:** Static  
- **Key Fields:** `point1`, `line`  
- **Notes:** A cleaned version (`08-connections-no-dupes.csv`) removes duplicate directional links

---

## 1.2 Project Steps Overview

1. **Loading** raw source files from `/sources/03-stations.csv` and `/sources/08-connections-no-dupes.csv`
2. **Cleaning** all station names (removing `"U-Bahnhof"`, trimming whitespace)
3. **Merging** station and connection datasets using cleaned station names with SQLite
4. **Adding** missing coordinates manually after the merge for ~12 stations with no `lat/lon` in the original dataset  
   → Coordinates will be sourced from Wikipedia, OpenStreetMap, and official BVG sources  
5. **Applying** reverse geocoding using the Nominatim API (`geopy`) to derive the `stadtteil` (district name)
6. **Exporting** final enriched station–line data to `ubahn_with_stadtteil.csv` for downstream use

---

## 1.3 Planned Transformations

- **Cleaning** and normalizing station names for consistent joins
- **Joining** `latitude` and `longitude` from the station dataset to each station in the line table
- **Adding** missing coordinates manually post-merge for stations such as:  
  *Friedrichstraße, Zoologischer Garten, Südkreuz, Hackescher Markt, Fehrbelliner Platz*, etc.  
- **Applying** reverse geocoding (via `geopy`) to assign each station to its respective `stadtteil`
- **Casting** postal code values back to `Int64` to fix float artifacts (e.g., `10787.0` → `10787`)

---

## 1.4 Output Schema (planned: `ubahn_with_stadtteil.csv`)

| station             | line | latitude | longitude | postcode | stadtteil      |
|---------------------|------|----------|-----------|----------|----------------|
| Zoologischer Garten | U9   | 52.5075  | 13.33417  | 10787    | Charlottenburg |
| Südkreuz            | U6   | 52.4750  | 13.36500  | 10829    | Schöneberg     |

---

## 1.5 `/sources` Directory Contents

### 📂 Files
- `03-stations.csv` – Cleaned list of Berlin U-Bahn stations with initial coordinates
- `08-connections-no-dupes.csv` – Unique U-Bahn connections between stations (directional deduped)

### 🔍 Notes
- Final enriched station file (with manually patched coordinates and reverse-geocoded district) is **not part of the public repo**
- All coordinate patches and reverse geocoding are applied in local scripts during transformation

---

## 📦 Final Outputs (Planned)

- `merged_ubahn_line.csv` – Station–line relationships + lat/lon + postcode (includes manual patches)
- `ubahn_with_stadtteil.csv` – Final enriched table with `stadtteil` added (for analysis and mapping)

---

## ⚠️ Known Limitations

- Reverse geocoding using Nominatim is subject to rate limits — `sleep(1)` is used between requests
- A small number of stations have no coordinates in the original dataset — resolved manually
- No caching used for API calls — failed responses may result in null values (`None`)

---

## ✍️ Author

**Simoun Asmar**  
Berlin U-Bahn Data Project · 2025
