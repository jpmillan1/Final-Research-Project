# Final Research Project — Part 2: Data Cleaning

This repository contains an end-to-end preprocessing workflow for cleaning, validating, and standardizing traffic datasets for use in transportation modeling. The project implements **Part 2: Data Cleaning** of the Architecture Alphabet framework and produces a unified SQLite database integrating two major data sources:

- **Trajectory data (`trajs`)** — segment-level crossing events.
- **Waypoint data (`waypoint`)** — high-frequency GPS pings.

The workflow ensures spatial consistency, removes invalid and error-coded records, standardizes timestamps across heterogeneous formats, and prepares the data for arc-based models, path-based analysis, tensor construction, and machine learning tasks.

---

## 🚦 Features

- Automatic CSV → SQLite ingestion with type inference  
- Cleaning of invalid SegmentId records  
- Removal of error-coded trajectories  
- Deduplication of waypoint timestamps  
- Filtering of fuzzed/outlier GPS data  
- Detection of missing GPS intervals using 3-second gap analysis  
- Full timestamp standardization (UTC, ISO, Unix → local time)  
- External validation using FHWA Data Cleaning and Fusion Tool (DCFT)  
- Output: unified, analysis-ready SQLite database

## 📁 Project Structure

``` 
├── src/
│   ├── csv_to_sqlite_processor.py
│   ├── basic_data_cleaner.py
│   └── time_standardization_processor.py
├── notebooks/
│   └── analysis.ipynb
├── Output/    # (ignored in .gitignore)
│   └── database/
│       └── unified_database.db
├── README.md
└── .gitignore
```


## ⚙️ Installation

```bash
pip install -r requirements.txt
```

Or install manually:

```bash
pip install pandas chardet pytz sqlite3
```





## ▶️ Usage

Run each processing stage in order:
python csv_to_sqlite_processor.py
python basic_data_cleaner.py
python time_standardization_processor.py

Or run the pipeline inside the Jupyter notebook.




📊 Validation
This project uses the FHWA Data Cleaning and Fusion Tool to validate:

- Spatial integrity

- Timestamp consistency

- Travel time confidence intervals (<0.1% outside federal bounds)

  


📚 Architecture Alphabet Context

This component supports:

A — Arc-based models (clean SegmentIds, consistent link flows)

B — Deep learning models (clean time series)

C — Path-based architectures (valid OD trajectories)

E — Tensor-based analysis (aligned temporal dimensions)

