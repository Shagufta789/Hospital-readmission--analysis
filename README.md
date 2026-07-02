# Hospital Readmission Analysis

## 📌 Overview
This project analyzes 10 years of patient discharge data from a hospital group to understand the factors driving patient **readmission**. The goal is to help the hospital's medical staff prioritize follow-up calls and monitoring for patients who are most likely to be readmitted.

The analysis was done as a consulting-style report, combining exploratory data analysis (EDA) with classification models to identify the strongest predictors of readmission.

## 🎯 Objectives
1. Identify the most common primary diagnosis by age group
2. Explore whether a diabetes diagnosis has a significant effect on readmission rates
3. Determine which groups of patients the hospital should prioritize for follow-up care based on their probability of readmission

## 📂 Dataset
- **Source:** [Diabetes 130-US hospitals for years 1999–2008](https://archive.ics.uci.edu/ml/datasets/Diabetes+130-US+hospitals+for+years+1999-2008) (UCI Machine Learning Repository)
- **Size:** 2,500 rows × 17 columns (no missing values or duplicates)
- **Key fields:**
  - `age` — patient age bracket
  - `time_in_hospital`, `n_procedures`, `n_lab_procedures`, `n_medications`
  - `n_outpatient`, `n_inpatient`, `n_emergency` — prior-year visit counts
  - `medical_specialty` — specialty of the admitting physician
  - `diag_1` / `diag_2` / `diag_3` — primary and secondary diagnoses
  - `glucose_test`, `A1Ctest` — diabetes-related lab results
  - `change`, `diabetes_med` — diabetes medication info
  - `readmitted` — target variable (yes/no)

> Note: The raw CSV is not included in this repo (see [Data Access](#-data-access) below).

## 🛠️ Tools & Libraries
- **Python** — pandas, NumPy
- **Visualization** — Matplotlib, Seaborn
- **Modeling** — scikit-learn (`DecisionTreeClassifier`, `RandomForestClassifier`, `GridSearchCV`)
- **Environment** — Jupyter Notebook

## 🧹 Data Cleaning
- Renamed columns for clarity (e.g., `diag_1` → `primary_diagnosis`, `A1Ctest` → `HbA1ctest`)
- Recoded age brackets into descriptive categories (e.g., `[70-80)` → `senior-old age`) and converted to a categorical dtype
- Converted diagnosis, test-result, and medication columns to categorical dtypes
- Verified there were no missing values or duplicate records

## 🔍 Analysis & Methodology
1. **Exploratory Data Analysis** — distribution of categorical and numeric variables, diagnosis breakdowns by age group, correlation heatmap
2. **Diabetes vs. readmission comparison** — split patients into diabetes vs. non-diabetes groups (based on primary/secondary/additional diagnosis) and compared readmission rates
3. **Predictive modeling** — encoded categorical features with one-hot encoding, then trained and compared:
   - A baseline **Decision Tree Classifier**
   - A **Random Forest Classifier**, further tuned via `GridSearchCV`
4. **Feature importance** — extracted from each model to identify which variables most strongly predict readmission

## 📈 Key Findings
- **Diagnosis patterns:** Circulatory conditions were the most common primary diagnosis across nearly all age groups (the exception being early-middle age, where "Others" ranked first).
- **Diabetes effect:** ~8,788 patients (about one-third) had diabetes as a primary, secondary, or additional diagnosis. Readmission rates were nearly identical between diabetic (~47%) and non-diabetic (~47%) patients — so diabetes alone does not appear to be a strong standalone driver of readmission.
- **Model performance:** The tuned Random Forest Classifier achieved the best result at **~61% accuracy** (vs. ~60% for the baseline Decision Tree), with modest precision (~0.61) and recall (~0.44).
- **Top predictors of readmission** (consistent across models): `n_inpatient`, `n_outpatient`, `n_lab_procedures`, `n_emergency`, and `n_medications`.

## ✅ Recommendations
The hospital should prioritize follow-up monitoring for patients who:
- Have had a **high number of prior inpatient admissions**
- Have had a **high number of prior outpatient visits**
- Are on **multiple medications**
- Have undergone **numerous lab procedures**
- Have had **prior emergency room visits**

These utilization patterns were more predictive of readmission than diagnosis type alone.

## 📂 Project Structure
```
hospital-readmission-analysis/
│
├── Hospital_readmission_analysis.ipynb   # Main analysis notebook
├── data/
│   └── hospital_readmissions.csv         # (not included — see Data Access)
└── README.md
```

## 🚀 How to Run
1. Clone this repository
   ```bash
   git clone https://github.com/your-username/hospital-readmission-analysis.git
   ```
2. Install dependencies
   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn jupyter
   ```
3. Place the dataset CSV in a `data/` folder (see below) and update the file path in the notebook if needed
4. Launch the notebook
   ```bash
   jupyter notebook Hospital_readmission_analysis.ipynb
   ```

## 📥 Data Access
The dataset used is derived from the [UCI Diabetes 130-US Hospitals dataset](https://archive.ics.uci.edu/ml/datasets/Diabetes+130-US+hospitals+for+years+1999-2008). Download it from the source above and place it in a `data/` folder before running the notebook.

## 🔮 Future Improvements
- Address class imbalance and low recall with resampling techniques (e.g., SMOTE) or class-weighting
- Try additional models (e.g., gradient boosting) to improve predictive performance
- Incorporate additional clinical variables if available (e.g., comorbidity indices)
- Build an interactive dashboard (Power BI/Tableau) summarizing key risk segments for hospital staff

## 👤 Author
**Your Name**
- GitHub: [@your-username](https://github.com/your-username)
- LinkedIn: [Your Name](https://linkedin.com)

## 📄 License
This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
