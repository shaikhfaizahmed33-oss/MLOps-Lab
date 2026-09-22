# Data Pipeline Documentation

## Dependency Graph
`collect` -> `preprocess` -> `features` -> `validate`

## Pipeline Stages
* **Collect**: Simulates ingesting raw data from an external source. 
  * Inputs: None
  * Outputs: `data/raw/iris_raw.csv`
* **Preprocess**: Handles missing values, removes duplicates, and corrects types. 
  * Inputs: `data/raw/iris_raw.csv`
  * Outputs: `data/processed/iris_preprocessed.csv`
* **Features**: Derives new, model-useful features (sepal_area, petal_area, length ratios, bins).
  * Inputs: `data/processed/iris_preprocessed.csv`
  * Outputs: `data/processed/iris_features.csv`
* **Validate**: Enforces schema and statistical checks before data flows to training.
  * Inputs: `data/processed/iris_features.csv`
  * Outputs: None (halts execution if failed)
  * Rules Enforced: specific expected columns present, no unexpected nulls, species in valid set, and numerical values within expected ranges.