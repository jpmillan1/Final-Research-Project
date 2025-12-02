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


## 🧰 FHWA Data Cleaning and Fusion Tool

The **Data Cleaning and Fusion Tool (DCFT)** developed by the U.S. DOT / FHWA is a standalone application designed to preprocess, clean, and validate large-scale transportation datasets. The tool can be used with **any compatible traffic dataset**, including connected-vehicle trajectories, GPS waypoints, detector readings, and map-matching data.

In this project, the DCFT application is included inside the folder **“Data Cleaning and Fusion Tool”** together with the original datasets needed to run it. These files are provided so that users can reproduce the exact initial conditions under which the unified database (`unified_database.db`) and map-matching results were generated.

The tool is publicly available at the official U.S. DOT repository: 
🔗 https://its.dot.gov/code/ 
🔗 https://github.com/usdot-jpo-codehub/data-cleaning-and-fusion-tool/tree/main  
or contact the FHWA Office of Operations for additional guidance.
Full references to the tool and documentation are provided at the end of this repository.

### ⚠️ Required configuration
To run the DCFT application correctly, users must manually adjust the **INPUT** and **OUTPUT** paths inside the tool interface.  
- **INPUT folder:** must point to the raw datasets included in this repository (`data_cleaning_fusion_datasets/`).  
- **OUTPUT folder:** must point to the desired directory where the tool will store the unified database, synthesized tables, and map-matching files (e.g., `Output/`).

### 🔎 Purpose within this project
The DCFT tool is used as the **first stage** of the data processing pipeline. It produces:
- `unified_database.db` (initial unified dataset)  
- `mapmatching/mapmatching.csv`  
- raw “before-cleaning” CSVs for trajectory, waypoint, and detector data  

These files are then further processed using the custom Python scripts found in the `src/` folder (`basic_data_cleaner.py` and `time_standardization_processor.py`).

This structure ensures full reproducibility and maintains consistency with FHWA’s official preprocessing standards.

## 🚀 Usage

This project requires running the **FHWA Data Cleaning and Fusion Tool (DCFT)** before executing any Python scripts. The usage workflow is:

### 1. Run the FHWA Data Cleaning and Fusion Tool
1. Download or use the version included in the **Data Cleaning and Fusion Tool** folder of this repository.  
2. Modify the **INPUT** folder to point to the raw datasets  
   (e.g., `data_cleaning_fusion_datasets/`).  
3. Modify the **OUTPUT** folder to specify where the tool should store results  
   (e.g., `Output/`).  
4. Run the application:
   - **Windows:** `Data Cleaning and Fusion Tool.exe`  
   - **macOS:** `Data Cleaning and Fusion Tool.app`  
5. The tool will generate:
   - `Output/database/unified_database.db`  
   - `Output/mapmatching/mapmatching.csv`  
   - Cleaned and synthesized tables  
   - Raw CSV copies for comparison (BEFORE)

### 2. Run the Python cleaning and analyses scripts
After the DCFT tool has produced the initial database:

```bash
python src/basic_data_cleaner.py
python src/time_standardization_processor.py
```

These scripts apply additional custom filtering, timestamp standardization, and database refinement.

### 3. Run the analysis notebook
Open the notebook for validation and visualization:

```bash
notebooks/analysis.ipynb
```

This produces the figures, metrics, convergence plots, and cross-source validation used in the final report.


## 🛠 Technologies Used
- **Python 3.10** — main programming language  
- **pandas** — data cleaning and manipulation  
- **SQLite3** — unified database storage  
- **matplotlib** — visualizations  
- **scikit-learn** — validation metrics (RMSE, R², MAE, MAPE)  
- **Jupyter Notebook** — exploratory analysis and documentation  



## 📁 Project Structure

``` 
├── src/
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

