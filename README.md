# Final Research Project — Part 2: Data Cleaning

This repository contains an end-to-end preprocessing workflow for cleaning, validating, and standardizing traffic datasets for use in transportation modeling. This project implements Part 2: Data Cleaning using the official FHWA Data Cleaning & Fusion Tool as the primary engine, supplemented with custom Python scripts for analysis, validation, and extended metrics.

Two major probe data sources are integrated:

Trajectory data (trajs) — segment-level crossing events

Waypoint data (waypoint) — high-frequency GPS pings

The workflow ensures spatial consistency, removes invalid and error-coded records, standardizes timestamps across heterogeneous formats, and prepares the cleaned data for arc-based models, path-based analysis, tensor construction, and machine-learning tasks.

---

## 🚦 Features

- Automatic CSV → SQLite ingestion with type inference  
- Removal of invalid SegmentId records (Package 2)
- Filtering of out-of-network and error-coded trajectory records
- Deduplication of waypoint timestamp entries (Package 1)  
- Detection (not removal) of fuzzed/missing waypoint GPS points
- Segment-level speed reconstruction using link lengths + travel times
- Full timestamp standardization (UTC, ISO, Unix → local time)
- Cross-source speed validation (Waypoint vs. Trajs)
- Unified, analysis-ready SQLite database aligned with FHWA preprocessing standards


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
1. Download or use the version included in the **Data Cleaning and Fusion Tool** folder of the provided repository "Acknowledgements" section. App.zip and Synthesize Data.zip, must be downloaded separately or obtained via Git clone to work properly. Downloading the entire repository directly will not be usable.  
2. Modify the **INPUT** folder to point to the raw datasets  
   (e.g., `data_cleaning_fusion_datasets/`).  
3. Modify the **OUTPUT** folder to specify where the tool should store results  
   (e.g., `Output/`).  
4. Run the application Package 1 and 2:
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


## 🛠 🧪 Technologies and Environment Used
- **Python 3.11.5** — This project was developed using the following environment configuration.
- **pandas** — data cleaning and manipulation  
- **SQLite3** — unified database storage  
- **matplotlib** — visualizations  
- **scikit-learn** — validation metrics (RMSE, R², MAE, MAPE)  
- **Jupyter Notebook** — exploratory analysis and documentation  



## 📁 Project Structure

``` 
├── CODE/
│   └── src/
│       └── basic_data_cleaning
│           ├── basic_data_cleaner.py
│           └── time_standardization_processor.py
│
├── notebooks/
│   ├── 1. Statistical Summary Comparison.ipynb
│   ├── 2. SPEED DISTRIBUTION ANALYSIS.ipynb
│   ├── 3. Corridor Speed Distribution Analysis (using map-matched waypoint).ipynb
│   ├── 4. cleaning_impacts_with_mapmatching.ipynb
│   ├── 5. speed_distribution_4panel_correct.ipynb
│   ├── 6. speed_distributions_waypoint_trajs_input_clean.ipynb
│   ├── 7. cross_source_speed_validation_links.ipynb
│   └── 8. data_completeness_waypoint_trajs.ipynb
│
├── data_cleaning_fusion_datasets/      # Input data of this project
│   ├── network/
│   ├── sensor/
│   ├── tmc_speed/
│   ├── waypoint/      # Waypoint data used in this project
│   └── trip path/     # Trip path data used in this project
│
├── Output/    # (ignored in .gitignore)
│   └── database/
│       └── unified_database.db
│
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




## 📜 Acknowledgements

This project makes use of the **Data Cleaning and Fusion Tool (DCFT)** developed as part of the Federal Highway Administration (FHWA) project entitled *“Data Fusion for Microsimulation Model Calibration”* under Contract Number 693JJ321D000010–693JJ322F00274N.

The DCFT application and source code are available publicly through the official U.S. DOT ITS CodeHub repository:

🔗 https://github.com/usdot-jpo-codehub/data-cleaning-and-fusion-tool

The outputs generated by this tool — including `unified_database.db`, `mapmatching.csv`, and the raw datasets provided in `data_cleaning_fusion_datasets/` — were used as the starting point for the custom cleaning, standardization, and analysis included in this project.

---

## 📑 Citing This Code

To track how this government-funded software is used, the FHWA requests that users acknowledge its Digital Object Identifier (DOI) in documentation and publications.

> **Digital Object Identifier:** https://doi.org/xxx.xxx/xxxx  
> *(Replace with the official DOI once published.)*

To cite this code in a report or publication, FHWA provides the following recommended citation:

> Federal Highway Administration. (2025). *Data Cleaning and Fusion Tool* (v0.1) [Source code].  
> Provided by ITS CodeHub through GitHub.com.  
> Accessed 2025-xx-xx from https://doi.org/xxx.xxx/xxxx.

When copying, adapting, or incorporating this code, please include the original URL from which it was retrieved as a comment in your code. Additional citation guidance is available in the  
🔗 **ITS CodeHub FAQ**: https://github.com/usdot-jpo-codehub/CodeHub-FAQ


## 📚 References

- Zhou, X., Taylor, J., & Zhang, Y. (2020). *Flow-through tensors: A unified computational graph architecture for multi-layer transportation network optimization.*

- Zhou, X., & Taylor, J. (2022). *Trajectory data-based traffic flow studies: A revisit.*

- Zhou, X., et al. (2021). *Virtual track networks: A hierarchical modeling framework for large-scale vehicle trajectory data.*

- Treiber, M., & Helbing, D. (2003). *Method for investigating intradriver heterogeneity using vehicle trajectory data: A Dynamic Time Warping approach.*

- **FHWA Data Cleaning and Fusion Guidelines**. Federal Highway Administration, U.S. Department of Transportation.  
  *(Corresponding tool and documentation available at: https://github.com/usdot-jpo-codehub/data-cleaning-and-fusion-tool)*

- Vaswani, A., Shazeer, N., Parmar, N., et al. (2017). *Attention Is All You Need.*  
  Advances in Neural Information Processing Systems (NeurIPS).

- Federal Highway Administration. (2025). *Data Cleaning and Fusion Tool* (v0.1) [Source code].  
  Provided by ITS CodeHub through GitHub.com. Accessed 2025-xx-xx from  
  https://github.com/usdot-jpo-codehub/data-cleaning-and-fusion-tool



