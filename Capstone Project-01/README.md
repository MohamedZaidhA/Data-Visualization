# Data Visualization Project Report

## Dataset Overview
This dataset is a comprehensive repository of academic faculty profiles across various Indian universities. It is intended to support large-scale data management, comparative analysis, and trend visualization within the higher education sector.

- **Total Records:** 15,500  
- **Total Attributes:** 15  
- **Data Characteristics:** Predominantly categorical (textual) with key numerical and binary indicators for quantitative analysis.

---

## Attribute Overview
The 15 attributes are grouped into four functional categories: **Identity**, **Institutional Context**, **Academic Profile**, and **Recognition**.

### 1) Identity & Reference
- **Vidwan-ID:** Unique alphanumeric identifier for each faculty member (useful for merges/lookups).
- **Name:** Full name of the faculty member.
- **Profile URL:** Link to the faculty’s digital profile for verification or further data collection.

### 2) Institutional Context
- **University:** Affiliated institution (e.g., IIT Madras, University of Delhi).
- **Department:** Academic division (e.g., Computer Science, Economics).
- **Position:** Designation (e.g., Assistant Professor, Dean, HOD).
- **Location:** Geographic placement of the university for spatial/regional analysis.

### 3) Academic & Professional Profile
- **Qualification:** General degrees held by the faculty.
- **Highest Qualification:** Terminal degree achieved (e.g., Ph.D., Post-Doc).
- **Expertise:** Research interests or specialization areas.
- **Years of Experience:** Numeric indicator of academic/professional experience.
- **Start Year:** Year entered the profession/current role (used for progression/hiring trends).

### 4) Achievement & Awards
- **Honours and Awards:** Text description of accolades.
- **Has Awards:** Binary indicator (**1 = Yes**, **0 = No**) for fast filtering and summaries.

---

## Project Objective (7 Parts)

1. **Data Cleaning**
   - Handle missing values in **Department** and **Qualification**.
   - Remove duplicates using **Vidwan-ID**.
   - Standardize **University** names for consistency.

2. **VLOOKUP**
   - Retrieve **Highest Qualification** using Vidwan-ID.
   - Retrieve **Years of Experience** using Name.
   - Retrieve **Department** using Vidwan-ID.

3. **Sorting**
   - Sort by **Years of Experience** (ascending).
   - Sort by **University Name** (A–Z).
   - Sort by **Start Year** (descending).

4. **Filtering**
   - Faculty with **> 10 years** experience.
   - Faculty with **Honours and Awards** / **Has Awards = 1**.
   - Faculty in **Computer Science** department.

5. **Data Validation**
   - **Start Year** must be between **1900 and 2024**.
   - **Has Awards** must be **0 or 1** only.
   - **Profile URL** must be a properly formatted URL (contains `http`).

6. **Creating Charts**
   - Bar chart: faculty count per university.
   - Pie chart: distribution of highest qualifications.
   - Line chart: hiring trend by year (Start Year).

7. **Interactive Dashboard**
   - KPIs: Total faculty, faculty with awards, average experience.
   - Filters: University, Department, Experience.
   - Interactive hiring/recruitment growth graph.

---

## 1. Data Cleaning

### 1.1 Handle Missing Values (Department & Qualification)
A multi-step imputation process was applied to address missing values:

- **Identification of Nullity:** Used Excel **Filter** to isolate blanks in **Department** and **Expertise**.
- **Pattern-Based Imputation (Flash Fill):**  
  When **Department** was missing but **Expertise** was present, **Flash Fill (Ctrl + E)** inferred likely departments based on provided examples (e.g., *Computer Science → Department: Computer Science*).
- **Reverse Inference:**  
  Used known department values to infer broad expertise categories where expertise was missing.
- **Standardization for Non-inferable Rows:**  
  Remaining blanks were filled with **"N/A"** to avoid “Blank” categories in Pivot Tables and dashboards.

### 1.2 Remove Duplicates (Vidwan-ID)
A controlled workflow was used to ensure Vidwan-ID uniqueness:

- **Visual Audit (Conditional Formatting):**  
  Applied a custom formula on Column A:  
  `=COUNTIF(A:A, A2) > 1`  
  This highlighted duplicates for manual review before deletion.
- **Automated Removal:**  
  Used **Data → Remove Duplicates** with **Vidwan-ID** selected as the sole key to remove redundant rows.

### 1.3 Standardize University Names
To avoid fragmentation in Pivot Tables, slicers, and charts, a three-phase standardization process was performed:

- **Text to Columns (Delimiter Split):**  
  Split entries containing city names separated by commas (e.g., *"Anna University, Chennai"*) and removed the city column.
- **Wildcard Replace (Suffix Cleaning):**  
  Used Find & Replace with wildcard logic to remove variable suffixes such as *(Autonomous)* or *(Deemed to be University)*, ensuring consistent grouping.
- **Manual Spelling/Abbreviation Audit:**  
  Corrected inconsistencies (e.g., *"Indn Inst of Tech"* → *"Indian Institute of Technology"*) into a single master naming convention.

**Outcome:** The dataset was refined to reduce noise, resolve inconsistencies, and prepare it for reliable analysis.

---

## 2. VLOOKUP (Search Interface)
A dynamic lookup interface was created to query the master dataset (**indian_faculty_dataset**) efficiently.

### 2.1 Highest Qualification (by Vidwan-ID)
```excel
=VLOOKUP(B4, indian_faculty_dataset!$A$2:$L$15501, 12, FALSE)
```
- Searches for Vidwan-ID in **B4**
- Returns value from the **12th** column (Highest Qualification)

### 2.2 Department (by Vidwan-ID)
```excel
=VLOOKUP(B4, indian_faculty_dataset!$A$2:$L$15501, 11, FALSE)
```
- Returns value from the **11th** column (Department)

### 2.3 Years of Experience (by Name)
```excel
=VLOOKUP(B29, indian_faculty_dataset!$B$2:$O$15501, 14, FALSE)
```
- Lookup starts from **Column B** (Name), since Vidwan-ID is not used here
- Returns **Years of Experience** by counting 14 columns to the right

**Key Implementation Notes**
- **Absolute references** (`$A$2:$L$15501`) keep ranges fixed when formulas are copied.
- **Exact match** (`FALSE`) prevents approximate matches in a unique-ID environment.
- Interface formatting: font turns **green** when a match exists and **red** when no record is found.

---

## 3. Sorting
Excel’s built-in Sort tool was used to structure views for analysis:

- **Experience-Based:** Sort by **Years of Experience (Ascending)** to analyze progression.
- **Institutional:** Sort by **University (A–Z)** for grouped viewing.
- **Chronological:** Sort by **Start Year (Descending)** to highlight recent recruitment.

---

## 4. Filtering
Excel Filters were applied to isolate key cohorts without modifying the master dataset:

- **Seniority Filter:** `Years of Experience > 10`
- **Recognition Filter:** `Has Awards = 1` (or non-empty Honours and Awards)
- **Department Filter:** `Department = "Computer Science"`

---

## 5. Data Validation
To keep future data entry consistent:

- **Start Year:** Whole number between **1900 and 2024**
- **Has Awards:** Dropdown limited to **0** or **1**
- **Profile URL:** Must contain `"http"` to maintain hyperlink integrity

---

## 6. Data Visualization (Charts)
Pivot Charts were produced to summarize institutional and academic trends:

- **Bar Chart (Faculty per University):** Compares faculty counts across universities.
- **Pie Chart (Highest Qualification Distribution):** Proportional breakdown (PhD, MTech, MSc, etc.).
- **Line Chart (Hiring Trend):** Recruitment pattern over time using **Start Year**.

---

## 7. Interactive Dashboard
An interactive dashboard was created for high-level monitoring and drill-down exploration:

### 7.1 Scorecards (KPIs)
- **Total Faculty Count**
- **Faculty with Awards** (sum of Has Awards)
- **Average Years of Experience**

### 7.2 Slicers (Dynamic Filters)
- University
- Department
- Experience range

### 7.3 Interactive Growth Graph
A line graph that updates automatically based on slicer selections to show recruitment trends by department and/or institution.

---

## Conclusion
This project transformed a raw dataset of ~15,500 faculty records into a structured, high-integrity academic analytics system using Excel.

**Key Outcomes**
- **Data Integrity:** Systematic cleaning and validation removed redundancy and improved consistency, creating a reliable source of truth.
- **Search Efficiency:** VLOOKUP-based search and structured sorting enabled near-instant retrieval.
- **Visual Intelligence:** Charts and an interactive dashboard converted categorical-heavy data into actionable insights on recruitment, qualification distribution, and institutional comparisons.

This framework is scalable for ongoing academic data management and supports fast, accurate decision-making for stakeholders.