# HIV Viral Load Cascade Analysis — Synthetic Program Dataset
### 🧩 Overview

This repository contains a complete HIV Viral Load (VL) Cascade Analysis workflow built using synthetic, non-identifiable patient-level data. The project mirrors real-world HIV program analytics, transforming raw records into actionable cascade insights across hospitals, districts, partners, and age groups.

This work demonstrates the intersection of public health expertise and modern data analytics, reflecting my ability to build data-driven solutions for HIV program monitoring and decision-making.

🔗 LinkedIn Post Update: https://www.linkedin.com/posts/andrew-nwachimereze-okebugwu-mbbs-mph-phd-6b429617_datascience-publichealth-hivprograms-activity-7403030618965610496-_OfU?utm_source=share&utm_medium=member_desktop&rcm=ACoAAANsTWgBIgGc3_UDSfNfwQH9Lji8-MnfyoQ

### 🎯 Objectives

- Generate analysis-ready patient datasets.
- Compute full VL cascade indicators (TX_CURR → Suppressed).
- Apply disaggregation across Hospital, District, Age Band, and Partner.
- Build combo visualizations:
  - Bars → TX_CURR, Eligible, Valid VL, Suppressed
  - Lines → VL Coverage %, VL Suppression %
- Produce insights to guide program optimization.

### Data for analysis:
https://docs.google.com/spreadsheets/d/1xy6Ry7BhzvC1kxhWSrfYvz2nsiw_KcSpwyBnmuRJQHA/edit?usp=sharing

### 🛠️ Tools & Technologies

Python: pandas, numpy

Visualization: matplotlib

Jupyter Notebook

Git/GitHub

Public Health Analytics (PEPFAR-aligned)


### 📁 Repository Structure

├── data/
│   └── synthetic_patient_data.csv
├── notebooks/
│   └── Viral Load Cascade Analysis.ipynb
├── visuals/
│   ├── cascade_general.png
│   ├── cascade_partner.png
│   ├── cascade_ageband.png
│   └── cascade_district.png
├── scripts/
│   └── vl_cascade_functions.py



### 📌 Code Snippets

Below are selected excerpts from the analysis.

#### 1️⃣ Loading Data & Preprocessing
patient = pd.read_csv("Data.csv")

patient["ART_start_date"] = pd.to_datetime(patient["ART_start_date"])
patient["Date_of_Viral_Load"] = pd.to_datetime(patient["Date_of_Viral_Load"])

####  2️⃣ Creating Age Groups

```
def assign_age_group(age):
    if age < 15:
        return "Child"
    elif 15 <= age <= 19:
        return "Adolescent"
    return "Adult"

patient["Age_Group"] = patient["Current_Age"].apply(assign_age_group)
```

#### 3️⃣ Computing Cascade Indicators
```
tx_curr = patient[patient["CurrentStatus_28"] == "Active"]

eligible = tx_curr[tx_curr["ART_start_date"].notna()]

cutoff = pd.Timestamp.today() - pd.DateOffset(years=1)
valid_vl = eligible[
    eligible["Date_of_Viral_Load"].between(cutoff, pd.Timestamp.today())
]

suppressed = valid_vl[valid_vl["Viral_load"] < 1000]
```

#### 4️⃣ Generating Summary Tables
```
summary = pd.DataFrame({
    "TX_CURR": len(tx_curr),
    "Eligible": len(eligible),
    "Valid VL Results": len(valid_vl),
    "Suppressed": len(suppressed)
})
```

#### 5️⃣ Visualization (Combo Chart Example)
```
fig, ax = plt.subplots(figsize=(10,6))

ax.bar(plot_df[group_col], plot_df["TX_CURR"])
ax.plot(plot_df[group_col], plot_df["VL Coverage (%)"], marker="o")

plt.title("VL Cascade by District")
plt.savefig("visuals/cascade_district.png", dpi=300)
```

### 📊 Charts & Visual Outputs

1️⃣ General VL Cascade

<img width="888" height="527" alt="Screenshot 2025-12-06 121503" src="https://github.com/user-attachments/assets/f74e8b0a-83f3-46ad-8eba-8e5bd3188dbe" />



2️⃣ Viral Load Cascade by Partner
<img width="1119" height="458" alt="Screenshot 2025-12-06 121404" src="https://github.com/user-attachments/assets/8dfe4489-2b3a-4fc6-a986-0289c30e2ef9" />

3️⃣ Viral Load Cascade by Age Bands
<img width="1138" height="454" alt="Screenshot 2025-12-06 121443" src="https://github.com/user-attachments/assets/7b22d826-6a5e-4bb4-a177-61cd8657c7f3" />

4️⃣ Viral Load Cascade by District
<img width="1133" height="499" alt="Screenshot 2025-12-06 122854" src="https://github.com/user-attachments/assets/22823b02-7659-43bb-a59b-be773ee4ed98" />


### 🔍 Key Findings (Synthetic Dataset)
⭐ General Cascade

VL coverage and suppression are low and high respectively among tested clients.

⭐ Age Bands

Adolescents show lower test coverage.

Children (<15 yrs) have fewer valid VL results.

⭐ Districts

District performance varies widely — clear opportunities for targeted supervision.

⭐ Partners

Mixed results among partners, demonstrating need for individualized technical support.

### 🧠 Insights

The eligibility → valid VL testing (VL coverage) gap is the most critical program barrier.

Suppression remains high once testing occurs, highlighting treatment success.

Strengthening VL sample collection workflows would significantly improve outcomes.

Adolescents require tailored adherence and testing interventions.

### ✔️ Recommendations

Implement automated VL eligibility reminders.

Improve adolescent-friendly clinic services.

Strengthen facility-level sample tracking.

Increase focus on underperforming districts.

Enforce data validation at the point of entry.

### 🚀 Next Steps

- Build an SQL + Python automated backend.

- Create a live Power BI dashboard.

- Integrate ML models for non-suppression prediction.

- Add SMS notification logic for clients due for VL.

👤 Author: Andrew Nwachimere-eze Okebugwu – Public Health Physician • HIV Specialist • Health Data Scientist

🔗 LinkedIn: https://www.linkedin.com/in/andrew-nwachimereze-okebugwu-mbbs-mph-phd-6b429617/

