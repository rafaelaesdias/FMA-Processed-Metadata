# FMA Metadata Processing Pipeline

## Description
This repository contains a data processing pipeline designed to clean, flatten, and standardize metadata from the **Free Music Archive (FMA)** dataset. The original FMA metadata files use complex multi-level headers, nested structures, and raw unparsed attributes that can be difficult to query directly. The objective of this project is to transform these raw inputs into clean, relational-ready CSV tables (`albums`, `artists`, `genres`, and `tracks`) for seamless integration into data analysis, database ingestion, or machine learning workflows.

## Source Data
The raw data originates from the official [FMA: A Dataset For Music Analysis](https://github.com/mdeff/fma) repository (Defferrard et al., ISMIR 2017). 

The input files must be placed inside the `fma_metadata/` directory:
* `genres.csv` — Genre hierarchy and track counts.
* `raw_albums.csv` — Unprocessed album-level metadata.
* `raw_artists.csv` — Unprocessed artist-level metadata and biographies.
* `raw_genres.csv` — Raw genre definitions and color codes.
* `raw_tracks.csv` — Unprocessed track-level metadata and licensing details.
* `tracks.csv` — Curated multi-index track metadata table from the FMA dataset.

## Processing
All data transformations are executed within the `process_fma_metadata.ipynb` Jupyter Notebook. Key steps include:
1. **Header Flattening:** Resolving multi-level headers (especially in `tracks.csv`) into standard, single-level column names.
2. **Feature Selection & Renaming:** Filtering out redundant columns and applying consistent snake_case naming conventions across all tables.
3. **Data Cleaning & Type Casting:** Handling missing values (`NaN`), parsing date strings into standard ISO formats, and ensuring IDs (`track_id`, `album_id`, `artist_id`, `genre_id`) are properly cast as integers.
4. **Text & List Normalization:** Cleaning string fields (stripping HTML tags or extraneous whitespace) and formatting nested list attributes (such as track genres and tags).
5. **Relational Integrity:** Ensuring foreign keys across tracks, albums, artists, and genres align properly before exporting to the `processed/` folder.

## Files

```text
├── fma_metadata/               # Input directory (raw FMA files)
│   ├── genres.csv
│   ├── raw_albums.csv
│   ├── raw_artists.csv
│   ├── raw_genres.csv
│   ├── raw_tracks.csv
│   └── tracks.csv
├── processed/                  # Output directory (cleaned CSVs)
│   ├── albums.csv
│   ├── artists.csv
│   ├── genres.csv
│   └── tracks.csv
└── process_fma_metadata.ipynb  # Data processing notebook
```

### Processed Outputs (`processed/`)
* **`albums.csv`**: Cleaned album information, including `album_id`, title, release date, album type, and metrics (listens, favorites, comments).
* **`artists.csv`**: Standardized artist profiles, containing `artist_id`, artist name, biography, location, active years, and engagement stats.
* **`genres.csv`**: Normalized genre taxonomy mapping `genre_id`, genre title, parent genre ID, and top-level genre classification.
* **`tracks.csv`**: Master track table linking `track_id` to `album_id` and `artist_id`, alongside track title, duration, bit rate, listens, date recorded, and associated genre IDs.

### Notebook
* **`process_fma_metadata.ipynb`**: End-to-end Jupyter Notebook that loads the raw CSVs from `fma_metadata/`, applies all cleaning and transformation steps, and exports the final datasets to `processed/`.

## Reproducibility

### Prerequisites
Ensure you have **Python 3.8+** installed along with the required data processing libraries:

```bash
pip install pandas numpy jupyter
```

### Instructions
1. **Clone the repository:**
   ```bash
   git clone <your-repository-url>
   cd <your-repository-folder>
   ```
2. **Download the source data:**
   Download `fma_metadata.zip` from the [official FMA repository](https://github.com/mdeff/fma) and extract the required CSV files into the `fma_metadata/` directory.
3. **Run the processing pipeline:**
   Launch Jupyter and execute all cells in the notebook:
   ```bash
   jupyter notebook process_fma_metadata.ipynb
   ```
   Alternatively, execute the notebook directly via the command line:
   ```bash
   jupyter nbconvert --to notebook --execute --inplace process_fma_metadata.ipynb
   ```
4. **Verify outputs:**
   Once execution completes, the four cleaned CSV files will be generated inside the `processed/` directory.