# Cafe Sales - A Data Cleaning Project with Analysis
### Dataset Source: [**kaggle**](https://www.kaggle.com/datasets/ahmedmohamed2003/cafe-sales-dirty-data-for-cleaning-training)
### Original Dataset Author: Ahmed Mohamed


## Overview
  This project focuses on cleaning and preparing a synthetic dataset representing cafe sales transactions using Excel Power Query, followed by analysis and visualization in Power BI. The goal was to transform inconsistent, incomplete, and messy data into a reliable format suitable for real-world business intelligence and decision-making.


## Understanding the Dataset
  Understanding the dataset's structure is crucial, as it guides how we approach data cleaning, especially when dealing with missing entries, duplicates, inconsistent data types, and messy values.

1. **Column Data Types (via Power Query):**  
   - **Transaction ID** - Text. 
   - **Item** - Text. 
   - **Quantity** - Any (Unassigned / Mixed type).		
   - **Price Per Unit** - Any (Unassigned / Mixed type).	
   - **Total Spent** - Any (Unassigned / Mixed type).
   - **Payment Method** - Text.
   - **Location** - Text.
   - **Transaction Date** - Any (Unassigned / Mixed type).


2. **Column relationship**
    - **Columns Affected:** Quantity, Price Per Unit, and Total Spent.  
    - **Relationships:**
        - Total Spent = Quantity * Price Per Unit  
        - Price Per Unit = Total Spent / Quantity  
        - Quantity = Total Spent / Price Per Unit
    - These formulas were used to verify data consistency and to detect any calculation errors or missing values.


3. **Column Values**
   - **Columns Affected:** Item, Quantity, Price Per Unit, Total Spent, Payment Method, Location, Transaction Date.
     - **"UNKNOWN"**, **"ERROR"**, and blank/null entries.


**Item Column:** 11 Unique Values Indentified.
<p align="Left">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/raw_item.png" width="25%" />
</p>


**Payment Method Column:** 6 Unique Values Identified.
<p align="Left">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/raw_payment_method.png" width="25%" />
</p>


**Location Column:** 5 Unique Values Identified.
<p align="Left">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/raw_location.png" width="25%" />
</p>


**Quantity Column:** 8 Unique Values Identified.
<p align="Left">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/raw_quantity.png" width="25%" />
</p>


**Price Per Unit Column:** 9 Unique Values Identified.
<p align="Left">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/raw_price_per_unit.png" width="25%" />
</p>


**Total Spent Column:** 20 Unique Values Identified.
<p align="Left">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/raw_total_spent.png" width="25%" />
</p>


**Transaction Date Column:** Since Transaction Date contains unique dates over all, we only identified the blank/null, "ERROR", and "UNKNOWN" values that are needed to be cleaned/removed.
<p align="Left">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/raw_transaction_date.png" width="25%" />
</p>


## Data Cleaning:
  To improve data quality and analysis accuracy, various cleaning steps were taken, including handling missing values, standardizing inconsistent entries, back-calculation in numerical columns and resolving formatting issues.


### Removed Duplicates  
   - **Tool used:** Data Tools - Remove Duplicates (Data Tab).
   - **Result:** No duplicate values found.


### Cleaning & Standardization for Text Columns
**Columns Affected:** Item, Payment Method, Location.

1. **Standardized unclean values** - Replaced **"UNKNOWN", "ERROR"**, and blank/null entries with a uniform category: **"Unknown"**.
    - This ensures consistency in grouping and avoids misleading aggregation in Power BI visuals.
2. **Text Formatting Enhancements:**
   - Applied **Trim** to remove leading and trailing spaces that could result in duplicate-looking values (e.g., " Cake" vs. "Cake").
   - Applied **Capitalize Each Word** to improve data readability and visual consistency (e.g., "cake" to "Cake").
3. **Data Type Formatting:**
   - **All Text Columns** - set to **Text**
4. **Tool Used** - All steps were performed in **Power Query** using built-in transformation tools.
     - **Replace Values** for standardizing entries (Transform Tab).
     - **Format** → **Trim** to clean up spaces.
     - **Format** → **Capitalize Each Word** to standardize casing.
     
  **Item Column:** 9 Unique Values Identified.  
  **Payment Method Column:** 4 Unique Values Identified.  
  **Location Column:** 3 Unique Values Identified.  
  (From Left to Right) 

<p align="center">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Processed/cleaned_item.png" width="21%" />
  &nbsp; &nbsp; &nbsp;
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Processed/cleaned_payment_method.png" width="25%" />
  &nbsp; &nbsp; &nbsp;
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Processed/cleaned_location.png" width="24%" />
</p>


### Cleaning, Back-Calculation & Formatting for Numerical Columns
**Columns Affected:** Quantity, Price Per Unit, Total Spent.
     
1. **Back-calculation of missing or invalid values** — entries labeled "ERROR", "UNKNOWN", or blank/null were recalculated if two valid values were present among the three numerical columns.

  Example Scenario:

  | Quantity   | Price Per Unit | Total Spent |
  |------------|----------------|-------------|
  |     1      | 2              | "ERROR"     |
  | 1          | (blank)        | 2           |
  | "UNKNOWN"  | 2              | 2           |

        
  - Using the known relationships:
    - Total Spent = Quantity * Price Per Unit
    - Price Per Unit = Total Spent / Quantity
    - Quantity = Total Spent / Price Per Unit

3. **Tool Used** - Back-calculations were performed using **Power Query**. *Transform Tab - Add Column - Custom Column*.
   - Changed the name into "Clean_Preferred_Column_Name" and used the following code/script depending on which column was being cleaned.
     - **Script/Code:** [Quantity](https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Excel%20-%20Power%20Query%20Script/quantity), [Price Per Unit](https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Excel%20-%20Power%20Query%20Script/price_per_unit), [Total Spent](https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Excel%20-%20Power%20Query%20Script/total_spent).   
     - **NOTE:** "ERROR", "UNKNOWN" and blank/null values left are from rows with no two valid values from two of the three numerical columns.
5. **Data Type Formatting:**
   - **Quantity** - set to **Whole Number**
   - **Price Per Unit, Total Spent** - set to **Decimal Number**
6. **Finalization:**
   - **Used Transform Tab** - **Replace Errors** to convert all error values to **null** for consistency.
   - Replaced original unclean columns with the newly processed Clean_ versions.   

  **Quantity Column:** 6 Unique Values Identified.  
  **Price Per Unit Column:** 7 Unique Values Identified.  
  **Total Spent Column:** 18 Unique Values Identified.  
  (From Left to Right)

<p align="center">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Processed/cleaned_quantity.png" width="25%" />
  &nbsp; &nbsp; &nbsp;
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Processed/cleaned_price_per_unit.png" width="25%" />
  &nbsp; &nbsp; &nbsp;
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Processed/cleaned_total_spent.png" width="24%" />
</p>


### Formatting Date Columns
**Columns Affected:** Transaction Date.

1. **Data Type Formatting:**
   - **Transaction Date** - set to **Date**
2. **Handled Errors & Missing Values:**
   - **Used Transform Tab** - **Replace Errors** to convert all error values to **null** for consistency.
3. **Tool Used** - All steps were executed using **Power Query**'s built-in data type and error handling tools. 

**Transaction Date:** Showing only the identified blank/null values due to unique date entries.

<p align="left">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Processed/cleaned_transaction_date.png" width="25%" />
</p>

### Final Validation

1. **Consistency Checks:**
   - Cross-checked calculated columns (e.g., Quantity, Price Per Unit, Total Spent) to confirm that relationships like Total Spent = Quantity × Price Per Unit hold true.
   - Ensured no logical contradictions remain after all cleaning steps.
2. **Null Review:**
   - Reviewed remaining null or "Unknown" entries to confirm they're either:
     - Truly missing and not recoverable, or
     - Appropriately categorized for analysis (e.g., "Unknown" payment method).
3. **Data Types Review:**
   - Verified that each column is assigned the correct data type:
     - Text for categorical data (e.g., Item, Location, Payment Method)
     - Decimal/Whole Number for numeric fields
     - Date for transaction timeline
4. **Row Count Check:**
   - Confirmed no rows were accidentally dropped or duplicated during the cleaning process.
5. **File Saved As:** cleaned_cafe_sales.xlsx


### Data Analysis and Visualization
To ensure a transparent, complete, and robust insights, the dataset was analyzed through a **split strategy**.
  - **General Attribute Analysis:**
    - The rest of the dataset—including these 460 entries—was retained for **non-time-based analyses**. These include metrics and patterns related to:
      - Item performance
      - Payment method distribution
      - Location-based breakdown
    - This approach preserved valuable categorical information found in rows with missing transaction dates, allowing deeper insights without unnecessary data loss.
  - **Time-Based Analysis:**
    - Rows with null or error entries in the Transaction Date column **(460 entries)** were excluded from **date-dependent analyses** such as monthly trends, seasonal patterns, and year-over-year comparisons. This ensured the visualizations were accurate and not skewed by missing time references.





