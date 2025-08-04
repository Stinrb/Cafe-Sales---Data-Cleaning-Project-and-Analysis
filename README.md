# Cafe Sales - A Data Cleaning Project
### Dataset Source: [**kaggle**](https://www.kaggle.com/datasets/ahmedmohamed2003/cafe-sales-dirty-data-for-cleaning-training)
### Original Dataset Author: Ahmed Mohamed


## Overview
  This project focuses on cleaning and preparing a synthetic dataset representing cafe sales transactions using Excel Power Query, followed by a small scale sales analysis and visualization using Power BI. The main goal was to transform inconsistent, incomplete, and messy data into a reliable format suitable for real-world business intelligence and decision-making.


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

  - **Time-Based Analysis:**
    - Rows with null or blank entries in the Transaction Date column **(460 entries)** were excluded from **date-dependent analyses** such as monthly trends comparisons. This ensured the visualizations were accurate and not skewed by missing time references.

  - **General Attribute Analysis:**
    - The 460 entries of null or blank values from Transaction Date were retained for **non-time-based analyses**. These include metrics and patterns related to:
      - Item performance
      - Payment method distribution
      - Location-based breakdown
    - This approach preserved valuable categorical information found in rows with missing transaction dates, allowing deeper insights without unnecessary data loss.


#### Categorical Column Distribution Analysis

  - Across the Item, Payment Method, and Location columns, the distribution shows relatively even if the "Unknown" entries were excluded. However, the "Unknown" category shows a disproportionately high percentage in both the Payment Method and Location columns.
    - Payment Method: 3178 (31.78%) "Unknown" entries.
    - Location: 3961 (39.61%) "Unknown" entries.
  
  - This result would significantly hinder data reliability and limit deeper insights on customer preferences, as well as on cafe operational performance (e.g., transactions, staff allocations, etc.). 
  
<p align="left">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Processed/cleaned_transaction_date.png" width="25%" />
</p>


#### Numerical Column Distribution Analysis

- Numerical Columns only shows a small portion of "Unknown" entries since it was remedied by using back-calculation during data cleaning. 

- **Distribution and Average**
  - **Quantity column** has an **even distribution from 1 to 5**, with an **average value of 3**.
  - **Price Per Unit column** shows a **large portion on 3 and 4 bins** with an even distribution from the rest of the bins (excluding "Unknown" entries). Price Per Unit also shows an **average value of 3**.
  - **Total Spent column** has a **large portion on 1-5 bin** which goes down as the value of the bins increases with "Unknown" having the lowest portion. Total Spent has an approximate **average value of 8**.
 
<p align="center">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Power%20BI%20Statis%20Visuals/quantity_bins.png" width="45%" />
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Power%20BI%20Statis%20Visuals/quantity_distribution.png" width="45%" />
</p>

<p align="center">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Power%20BI%20Statis%20Visuals/price_per_unit_bins.png" width="45%" />
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Power%20BI%20Statis%20Visuals/price_per_unit_distribution.png" width="45%" />
</p>

<p align="center">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Power%20BI%20Statis%20Visuals/total_spent_bins.png" width="45%" />
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Power%20BI%20Statis%20Visuals/total_spent_distribution.png" width="45%" />
</p>


#### Sales and Item Performance

This section provides a brief snapshot of sales performance. The primary objective of this project is **data cleaning**, so the analysis here remains intentionally light. A more in-depth exploration of trends and metrics can be explored in a future reporting-focused project.

Let's answer a few business targeted questions:
1. Which item is the most popular?
2. Is there a sales trend?

- **Item Performance**
  - **Salad** generated the **highest total revenue** while **Cookie** shows the **lowest** sale.
  - **Coffee** recorded the **most quantity sold** among all items with **Cookie** having the **least** sold.
  - The cafe customers spend an **average** of **2.95 per item** and **8.93 per transaction**.
    
<p align="center">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Power%20BI%20Statis%20Visuals/item_revenue.png" width="45%" />
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Power%20BI%20Statis%20Visuals/item_quantity_sold.png" width="45%" />
</p>

<p align="center">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Power%20BI%20Statis%20Visuals/table.png" width="60%" />
</p>


- **Sales Trend**
  - Sales started strong in **January**, but unsually **dipped** the following month of **February**, which also is the **lowest sales** produced that year (2023). Sales performance recovered from the rest of the year with **June** having the **highest peak of sales**, followed by **October**.
 
<p align="center">
  <img src="https://github.com/Stinrb/Cafe-Sales---Data-Cleaning-Project-and-Analysis/blob/main/Visualizations/Power%20BI%20Statis%20Visuals/monthly_sales_trend.png" width="100%" />
</p>


### Final Key Summary

  1. **Improve Data Entry Accuracy**  
      - Cafe data collection process needs to be reviewed to avoid "Unknown" entries that can affect the reliability and completeness of the data. This can produce an inconsistencies in insights which can lead to flawed business decisions.
  2. **Leverage Popular & High-Revenue Items**  
      - Since Coffee records the most quantity sold, it can mean that it is a staple purchase and can be bought with low price, and since Sald generates the highest item revenue, this presents a sale opportunity by bundling both items seen in combo deals. This can help in boosting the overall Cafe sales in the future.
