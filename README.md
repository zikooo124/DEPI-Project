# 🫀 Heart Disease Risk Assessment

> A comprehensive data analysis project to identify key risk factors for heart disease using real-world data from 297,396 patients.

---

## 📌 Project Overview

Heart disease is one of the leading causes of death worldwide. This project aims to analyze patient data to identify the most significant risk factors contributing to heart disease, enabling early identification of at-risk individuals.

**Tools Used:**
-  **![Excel](https://img.shields.io/badge/Excel-217346?style=flat&logo=microsoftexcel&logoColor=white)** — Data Cleaning & Modeling
-  **![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=flat&logo=postgresql&logoColor=white)** — SQL Analysis
-  **![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)** — Visualization

   
---

## 👥 Team Members

| Name |
|------|
| Albaraa Ehab Elzeftawy |
| Esraa Hamada |
| Mohamed Sherif Mohamed |
| Ziad Hesham Mohamed |

---

## 📂 Project Structure

```
Heart-Disease-Risk/
│
├── 📁 Data/
│   └── heart_2020_cleaned.csv          # Original dataset from Zenodo
│
├── 📁 Power Query/
│   └── F-Project.xlsx                  # Cleaned & modeled data
│
├── 📁 SQL/
│   └── F-Project.sql            # All PostgreSQL queries
│
├── 📁 Dashboard/
│   └── F-Project.pbix                  # Power BI Dashboard file
│
└── 📁 Presentation/
    └── Final-Project.pptx              # Final presentation
```

---

## 🗂️ Dataset

- **Source:** [Heart Disease Dataset — Zenodo](https://zenodo.org/records/15336526)
- **Records:** 297,396 patients
- **Features:** 18 original columns including demographic and health data

**Key original features:**
| Feature | Description |
|---------|-------------|
| HeartDisease | Target variable (Yes/No) |
| BMI | Body Mass Index |
| Smoking | Smoking status |
| AlcoholDrinking | Alcohol consumption |
| PhysicalActivity | Physical activity status |
| SleepTime | Average sleep hours |
| AgeCategory | Age group |
| Race | Race/ethnicity |
| Sex | Gender |
| Diabetic | Diabetes status |
| Stroke | Stroke history |
| GenHealth | General health self-assessment |
| KidneyDisease | Kidney disease status |
| SkinCancer | Skin cancer status |

---

## 🧹 Data Cleaning (Power Query)

All cleaning was performed using Power Query in Excel:

- ✅ Removed duplicate rows
- ✅ Removed blank rows
- ✅ Fixed data types
- ✅ Standardized categorical values (Yes/No)
- ✅ Filtered unrealistic BMI values (kept 15–60)
- ✅ Filtered unrealistic SleepTime values (kept 4–16 hours)
- ✅ Reordered columns for clarity

---

## 🧠 Data Modeling (Feature Engineering)

New columns created to enrich analysis:

| New Column | Logic | Values |
|-----------|-------|--------|
| `Age_Group` | Grouped AgeCategory | 18-30 / 31-45 / 46-60 / 60+ |
| `BMI_Category` | Based on BMI value | Underweight / Normal / Overweight / Obese |
| `Sleep_Category` | Based on SleepTime | Short Sleep / Normal Sleep / Long Sleep |
| `Lifestyle` | Based on Smoking + PhysicalActivity + AlcoholDrinking | Healthy / Low Risk / Moderate Risk / High Risk Lifestyle |
| `Chronic_Count` | Count of chronic conditions (Diabetic + Asthma + KidneyDisease + SkinCancer) | 0–4 |
| `Health_Burden` | Based on Chronic_Count | No Risk / Medium / High |
| `Unhealthy_Days` | PhysicalHealth + MentalHealth | 0–60 |
| `Unhealthy_Level` | Based on Unhealthy_Days | Healthy / Moderate / Unhealthy |
| `Heart_Risk_Score` | Weighted scoring system | 0–12 |
| `Heart_Risk` | Based on Heart_Risk_Score | Low Risk / Medium Risk / High Risk |

### Heart Risk Score Formula

```
Score = 
  BMI_Category   (Obese=2, Overweight=1)
+ Sleep_Category (abnormal=1)
+ Lifestyle      (High Risk=2, Moderate=1)
+ Age_Group      (60+=2, 46-60=1)
+ Chronic_Count  (≥2 → 2pts, =1 → 1pt)
+ Stroke         (Yes=3)
+ DiffWalking    (Yes=2)

Thresholds:
  0–2  → Low Risk
  3–5  → Medium Risk
  6–12 → High Risk
```

---

## 🐘 SQL Analysis (PostgreSQL)

All analysis was performed in PostgreSQL across 5 sections:

### 1. Overview Analysis
```sql
SELECT HeartDisease, COUNT(*) AS total_people,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 2) AS percentage
FROM project.health
GROUP BY HeartDisease;
```

### 2. Demographic Analysis
- Heart Disease Rate by Age Group
- Heart Disease Rate by Sex
- Heart Disease Rate by Race
- Multi-factor: Sex × Age Group

### 3. Lifestyle & Behavioral Analysis
- Heart Disease Rate by Lifestyle
- Heart Disease Rate by Smoking
- Heart Disease Rate by Physical Activity
- Heart Disease Rate by Alcohol Drinking

### 4. Health Metrics Analysis
- Heart Disease Rate by BMI Category
- Heart Disease Rate by Sleep Category
- Heart Disease Rate by General Health
- Heart Disease Rate by Unhealthy Level

### 5. Chronic Conditions Analysis
- Heart Disease Rate by Chronic Count
- Heart Disease Rate by Health Burden
- Heart Disease Rate by Diabetic Status
- Heart Disease Rate by Stroke
- Heart Disease Rate by Difficulty Walking

---

## 📊 Key Findings

| Finding | Value |
|---------|-------|
| Overall Heart Disease Rate | **8.94%** |
| Males 60+ Disease Rate | **23.3%** |
| High Health Burden Rate | **40.3%** |
| Stroke Patients Rate | **35.2%** |
| Smokers vs Non-Smokers | **12.43% vs 6.41%** |
| Poor General Health Rate | **33%** |
| Diabetic Patients Rate | **19.8%** |

---

## 📈 Power BI Dashboard

Interactive dashboard with 4 pages connected directly to PostgreSQL database:

**Page 1 — Overview**
- KPI Cards: Heart Disease Cases, Total People, Disease Rate, High Risk Count, Avg Sleep Hours
- Avg BMI & Chronic Count (Gauge)
- GenHealth Assessment (Bar Chart)
- Physical Activity Risk (Bar Chart)
- Lifestyle Risk (Bar Chart)
- Heart Disease Distribution (Donut Chart)
- Risk Level Distribution (Donut Chart)
- Interactive Slicer: Sex

**Page 2 — Overview 2**
- SleepTime Risk (Bar Chart)
- Risk by Race (Bar Chart)
- KidneyDisease Risk (Donut Chart)
- Lifestyle Risk (Bar Chart)
- Sex and SleepTime Relation (Area Chart)
- BMI Category Distribution

**Page 3 — Demographics**
- Heart Disease Rate by Age Group (Bar Chart)
- Heart Disease Rate by Sex (Pie Chart)
- Heart Disease Rate by Race (Bar Chart)
- Heart Disease Rate by Age and Sex (Multi-factor Bar Chart)
- Interactive Slicers: Sex & Age Group

**Page 4 — Risk Factors**
- Heart Disease Rate by Lifestyle (Bar Chart)
- Heart Disease Rate by BMI (Bar Chart)
- Heart Disease Rate by Sleep (Bar Chart)
- Heart Disease Rate by Smoking (Pie Chart)
- Heart Disease Rate by Stroke (Bar Chart)
- Heart Disease Rate by Diabetic (Bar Chart)

---

## 💡 Recommendations

1. **Target 60+ Age Group** — Routine cardiac screening should be prioritized
2. **Stroke & Mobility Monitoring** — Close cardiac follow-up is essential
3. **Promote Physical Activity & Healthy BMI** — Lifestyle interventions have measurable impact
4. **Smoking Cessation Programs** — Smokers show nearly 2× the disease rate
5. **Manage Chronic Conditions** — Integrated management reduces overall risk
6. **Early Diabetes Intervention** — Early glucose control significantly lowers cardiac complications

---

## 🖼️ Screenshots

### Dashboard — Overview
![Overview](https://github.com/user-attachments/assets/f02dc08e-0c9e-4192-b64d-2470f1f2b86a)

### Dashboard — Overview 2
![Overview2](https://github.com/user-attachments/assets/360acd44-1912-449f-9b51-f594ba195a48)

### Dashboard — Demographics
![Demographics](https://github.com/user-attachments/assets/bb8b4e59-86c2-4cfc-ac9b-c05cda30cd0d)

### Dashboard — Risk Factors
![Risk Factors](https://github.com/user-attachments/assets/43ec9a6d-b9ff-406a-9496-5cd4a4259590)

---

## 📄 License

This project is for educational purposes.

---

*Data Analysis Project • Power Query • PostgreSQL • Power BI*
