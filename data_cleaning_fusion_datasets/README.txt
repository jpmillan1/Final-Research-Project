## 📁 Data Used in This Project

The data used in this project comes from the official **FHWA Data Cleaning and Fusion Tool** repository:

🔗 https://github.com/usdot-jpo-codehub/data-cleaning-and-fusion-tool

To reproduce the data cleaning process, users must download the raw datasets provided by FHWA. These files are located inside the archive:

Synthesize Data.zip


within the FHWA repository.

### Required datasets for this project
From **Synthesize Data.zip**, the following files are needed:

- `trajs.csv` — segment-level trajectory crossing events  
- `waypoints.csv` — high-frequency GPS waypoint records  

These two files serve as the **input** to the FHWA DCFT application, which generates:

- `unified_database.db`  
- map-matching results  
- synthesized datasets  
- raw “BEFORE” folders

Once the FHWA tool has produced the unified database, the Python scripts in this project (`basic_data_cleaner.py` and `time_standardization_processor.py`) perform additional cleaning, refinement, and timestamp standardization.

Users must ensure that the **INPUT** and **OUTPUT** folders in the FHWA tool are correctly configured to point to the downloaded datasets before running the application.
