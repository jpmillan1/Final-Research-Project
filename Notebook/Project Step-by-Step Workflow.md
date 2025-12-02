# Project Step-by-Step Workflow

This document summarizes the exact workflow used to complete **Part 2: Data Cleaning** of the Architecture Alphabet framework. It explains how the FHWA tool and the custom Python scripts were used sequentially to generate the final cleaned and validated dataset.

---

# ⚙️ Installation and Notebook Usage

This project was developed in Python and uses both modular scripts (`.py`) and Jupyter notebooks (`.ipynb`) for analysis and validation.  
To ensure reproducibility, follow the installation steps below.

---

## ✅ Requirements
- Python 3.8 or later  
- pip (Python package manager)  
- Jupyter Notebook or JupyterLab  
- (Optional) Visual Studio Code with the Python extension  
- (Optional) Anaconda Distribution (Jupyter included, but not required)

---

## 📦 Install Dependencies

Install all dependencies using the provided `requirements.txt`:

```bash
pip install -r requirements.txt

or manually install
pip install pandas chardet pytz matplotlib scikit-learn jupyter




## 1. Run the FHWA Data Cleaning and Fusion Tool (DCFT)

The first stage of the project uses the official **FHWA Data Cleaning and Fusion Tool**, which performs initial preprocessing on the raw datasets. The tool:

- Imports and formats raw CSV datasets  
- Applies FHWA’s spatial cleaning and consistency checks  
- Performs default trajectory map-matching  
- Generates synthesized tables (e.g., `trajs_synthesized`)  
- Produces `unified_database.db`  
- Creates the `mapmatching/mapmatching.csv` file  
- Exports raw BEFORE-cleaning datasets in `data_cleaning_fusion_datasets/`

These files serve as the foundation for all subsequent Python-based cleaning steps.

---

## 2. Apply Custom Python Cleaning Pipeline

The second stage consists of running the custom Python scripts located in the `src/` folder. These scripts refine, standardize, and validate the data produced by the FHWA tool.

### **`basic_data_cleaner.py`**
This module performs advanced cleaning on the FHWA output:

- Removes out-of-corridor records (SegmentId validation)  
- Eliminates all `ErrorCodes` in trajectory data  
- Removes fuzzed GPS points  
- Deduplicates waypoints  
- Detects missing GPS intervals using a 3-second gap rule  
- Updates cleaned tables inside `unified_database.db`

### **`time_standardization_processor.py`**
This script enforces temporal consistency across all datasets:

- Converts UTC and ISO timestamps to local time  
- Standardizes formats to `YYYY-MM-DD HH:MM:SS`  
- Adds `local_time` columns where needed  
- Ensures alignment of timestamps across `trajs`, `waypoint`, `lane_readings`, and `Readings`

---

## 3. Perform Data Analysis and Validation

The final stage is implemented in the Jupyter notebook:

### **`notebooks/`**
This notebook computes key metrics and produces visual validation outputs:

- BEFORE vs AFTER row counts  
- Convergence plots for `trajs`  
- Cleaning performance metrics:
  - Spatial validity  
  - Error-coded row removal  
  - Fuzzed-point filtering  
  - Timestamp consistency  
- Cross-source speed validation:
  - Comparing waypoint speeds (from `mapmatching.csv`)  
    with segment-level trajectory speeds (`CrossingSpeedMph`)  
  - RMSE, MAE, MAPE, R² calculation

Because **waypoint map-matching is optional in the FHWA tool** and was **not applied in this project**, the validation compares raw GPS waypoints to cleaned trajectory speeds. The resulting variability is expected and reflects measurement differences rather than cleaning errors.

---

## ✔ Summary

This project follows a three-stage cleaning pipeline:

1. **FHWA DCFT** generates the initial database and map-matching outputs.  
2. **Custom Python scripts** refine, filter, and standardize the FHWA outputs.  
3. **Jupyter Notebook analysis** validates the final cleaned dataset using convergence, performance metrics, and cross-source comparisons.

This step-by-step workflow reproduces the complete data-cleaning methodology required for Part 2 of the Architecture Alphabet framework.
