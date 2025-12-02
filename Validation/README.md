## ✅ Validation

This project includes several forms of validation to ensure that the cleaning pipeline behaves correctly and produces consistent and interpretable outputs.

### 1. Example Outputs for Verification
The notebook `analysis.ipynb` provides example outputs that can be used to verify correctness, including:
- BEFORE vs AFTER row counts for `trajs` and `waypoint`
- Convergence plots showing how many records remain after each cleaning stage
- Error-code removal statistics
- Fuzzed-point filtering results
- Timestamp ranges before and after standardization

These outputs provide a visual and quantitative check that each stage of the pipeline behaves as intended.

### 2. Unit-Style Checks for Key Functions
Although this project does not require formal unit tests, several unit-style validations are embedded directly into the code:
- **SegmentId validation:** checks whether each `trajs` record belongs to a valid link  
- **ErrorCode validation:** confirms that no rows with non-empty `ErrorCodes` remain  
- **Waypoint deduplication:** verifies that `(journey_id, capture_time)` pairs contain no duplicates  
- **Fuzzed-point removal:** checks the exact number and percentage of `fuzzed_point = '1'` rows  
- **Timestamp standardization:** ensures `local_time` exists and is formatted as `YYYY-MM-DD HH:MM:SS`

These checks behave like lightweight unit tests and validate the correctness of the core cleaning steps.

### 3. Validation Scripts with Expected Results
Several scripts in the repository produce validation-ready tables and figures with expected outputs:

- **`analysis**  
  Computes:
  - Spatial validity percentage  
  - Error-code removal count  
  - Fuzzed-point percentage  
  - Timestamp consistency  
  - Cross-source speed validation (RMSE, MAE, MAPE, R²)

- **`convergence_analysis.py` (or equivalent block inside the notebook)**  
  Produces:
  - `convergence_counts.csv`  
  - `convergence_summary.csv`  
  - `convergence_rows.png`  
  - `convergence_pct_removed.png`

Expected results include:
- `trajs_synthesized → trajs` reduction from ~8.3M to ~47k  
- ~0.56% spatial validity prior to filtering  
- 100% removal of error-coded records  
- ~8.96% fuzzed waypoint points removed  
- R² ≈ –1.02 when comparing raw waypoint speeds with cleaned trajectory speeds

These outputs confirm that the cleaning steps executed successfully and that differences between data sources originate from measurement characteristics rather than processing errors.

