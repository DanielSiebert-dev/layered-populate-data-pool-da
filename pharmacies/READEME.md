## A. Data Source Discovery
- **Main source:** OpenStreetMap (OSM) via the OSMnx Python library.
- **Reason:** Open, free, and continuously updated. Contains location, address, and other details for all banks in Berlin.
- **Data type:** Dynamic (queried via API using `amenity=pharmacy` filter).
- **Update frequency:** Continuous.
- **Extra sources:** Berlin Open Data Portal (optional enrichment).

---

## B. Selected Columns (16 total)
1. pharmacy_id 
2. name  
3. operartor  
4. street  
5. housenumber  
6. postcode  
7. suburb 
8. phone
9. website
10. openinghours
11. dispensing
12. wheelchair_acessible 
13. latitude  
14. longitude  
15. district 
16. district_id
 
---

## C. Planned Schema
- Table name: `pharmacies`
- Data types chosen to match expected content (text, float).
- `latitude` and `longitude` extracted from geometry.
- `district` will be added later via geopy.geocoders /Nominatim.

---

## D. Transformation Plan
1. Fetch data from OSM (`amenity=pharmacy`, Berlin).
2. Convert column names to snake_case.
3. Standardize column names and types
4. Drop irrelevant / redundant columns
5. Keep missing values
6. Normalize categories 
7. Opening hours normalization
8. Enrich with district and district_id information.
9. Save cleaned and transformed dataset (CSV).

---
  
## E. Populate Database
1. Create empty table 'pharmacies'.
2. Send data frame to the database
3. Query data

---

## F. Files in `/sources` 
- **pharmacies_raw.geojson** — raw data with geometry.
- **pharmacies_raw.csv** — raw data without geometry.
- **final_pharmacies_with_districts.csv** - data enriched with district information.

---

## G. Files in `/scripts` 
- **pharmacies_data_transformation.jpynb** — jupyter notebook

---

## H. Files in `/pharmacies` 
- **README.md** — this file.

