# Data Assessing & Cleaning: Clinical Trial Dataset

This repository contains a data wrangling and cleaning project focused on programmatically and visually assessing and cleaning clinical trial datasets using **pandas** and **numpy** in Python.

---

## Section Navigation

- [Project Overview](#project-overview)
- [Key Data Issues Identified](#key-data-issues-identified)
- [Data Cleaning Order Protocol](#data-cleaning-order-protocol)
- [Key Cleaning Procedures Executed](#key-cleaning-procedures-executed)
- [Execution Instructions](#execution-instructions)

---

## Project Overview

The objective of this project is to take raw, uncleaned clinical trial data distributed across multiple tables, identify data quality (dirty data) and tidiness (messy data) issues, and systematically wrangle the dataset into a clean, unified structure.

### Datasets Included
- **`patients.csv`**: Baseline demographic and contact information (patient names, addresses, physical metrics).
- **`treatments.csv`**: Treatment records detailing drug administration (Auralin vs. Novodra), HbA1c levels, and dosage information.
- **`treatments_cut.csv`**: Additional treatment records sharing the same schema as `treatments.csv`.
- **`adverse_reactions.csv`**: Reported adverse reactions associated with trial participants.

---

## Key Data Issues Identified

### 1. Data Quality Issues (Dirty Data)

#### **`patients` Table**
- **Inaccurate Name**: Misspelled name `'Dsvid'` instead of `'David'` (`patient_id = 9`).
- **Inconsistent State Formatting**: The `state` column contains a mix of full state names and standard two-letter postal abbreviations.
- **Invalid Zip Codes**: Zip code values contain four-digit entries due to missing leading zeros or float conversion issues.
- **Missing Data (Completeness)**: 12 patients are missing address, city, state, zip code, country, and contact details.
- **Incorrect Data Types**: Columns `assigned_sex`, `zip_code`, and `birthdate` have improper data types assigned.
- **Duplicate Entries**: Duplicate entries present under the name "John Doe".
- **Outliers and Inaccurate Metrics**:
  - One record indicates an unrealistic weight of **48.8 lbs**.
  - One record indicates an unrealistic height of **27 inches**.

#### **`treatments` & `treatments_cut` Tables**
- **Inconsistent Casing**: `given_name` and `surname` values are formatted entirely in lowercase.
- **Structural Characters in Values**: Trailing unit characters (`'u'`) are present in dosage columns (`auralin` and `novodra`).
- **Improper Missing Value Indicators**: Unused doses are represented with dash strings (`'-'`) rather than explicit null values (`NaN`).
- **Uncalculated Values**: Missing values exist within the `hba1c_change` column.
- **Duplicate Records**: Duplicate entry found for participant "Joseph Day".
- **Inaccurate Data Entry**: Record contains an `hba1c_change` value entered as `9` instead of `4`.

#### **`adverse_reactions` Table**
- **Inconsistent Casing**: `given_name` and `surname` values are formatted entirely in lowercase.

---

### 2. Structural Tidiness Issues (Messy Data)

- **`patients` Table**: The `contact` column violates tidy data principles by bundling two distinct variables (phone number and email address) into a single string.
- **`treatments` & `treatments_cut` Tables**:
  - Dosage columns (`auralin`, `novodra`) bundle two variables into one cell (starting dose and ending dose, e.g., `29u - 36u`).
  - Treatment records are split across two redundant tables (`treatments` and `treatments_cut`) and should be merged.
- **`adverse_reactions` Table**: This dataset functions as an attribute of the treatment observation and should be merged into the main treatment dataset rather than maintained as an isolated table.

---

## Data Cleaning Order Protocol

To preserve data integrity, operations follow a structured sequence:

1. **Completeness**: Resolve missing values and missing records first.
2. **Tidiness**: Restructure tables into tidy data formats (one variable per column, one observation per row).
3. **Validity**: Correct data types, structural constraints, and value bounds.
4. **Accuracy**: Eliminate inaccurate values, typos, and duplicate records.
5. **Consistency**: Standardize text casing, state representations, and missing value representations.

---

## Key Cleaning Procedures Executed

1. **Information Extraction**: Applied regular expressions (`regex`) to parse `contact` strings into explicit `phone` and `email` columns.
2. **Derived Metrics**: Imputed missing `hba1c_change` values by computing `hba1c_start - hba1c_end`.
3. **Table Consolidation**: Merged `treatments` and `treatments_cut` DataFrames into a single, comprehensive dataset.
4. **Type Conversion**: Recast data types (`zip_code` to padded string formats, `birthdate` to datetime objects, categorical columns to appropriate categorical types).

---

## Execution Instructions

1. Ensure Python 3.x along with `pandas` and `numpy` are installed in your environment.
2. Place `patients.csv`, `treatments.csv`, `treatments_cut.csv`, and `adverse_reactions.csv` in the working directory.
3. Open `Data_Assessing_and_Cleaning.ipynb` in [Google Colab](https://colab.research.google.com/drive/1d65BOzovlyhiyFKA8VTaHKzoa1u0MEgp) or Jupyter Notebook.
4. Execute cells sequentially to run the assessment and programmatic cleaning steps.
