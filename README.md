# HIV Viral Load Cascade Analysis — Synthetic Program Dataset
### 🧩 Overview

This repository contains a complete HIV Viral Load (VL) Cascade Analysis workflow built using synthetic, non-identifiable patient-level data. The project mirrors real-world HIV program analytics, transforming raw records into actionable cascade insights across hospitals, districts, partners, and age groups.

This work demonstrates the intersection of public health expertise and modern data analytics, reflecting my ability to build data-driven solutions for HIV program monitoring and decision-making.

🔗 LinkedIn Post Update: (insert link after publishing)
https://www.linkedin.com/in/YOUR-LINKEDIN-USERNAME/ (placeholder)

### 🎯 Objectives

- Generate analysis-ready patient datasets.
- Compute full VL cascade indicators (TX_CURR → Suppressed).
- Apply disaggregation across Hospital, District, Age Band, and Partner.
- Build combo visualizations:
  - Bars → TX_CURR, Eligible, Valid VL, Suppressed
  - Lines → VL Coverage %, VL Suppression %

- Export clean PNG graphics for dashboards and reporting.
- Produce insights to guide program optimization.



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

def assign_age_group(age):
    if age < 15:
        return "Child"
    elif 15 <= age <= 19:
        return "Adolescent"
    return "Adult"

patient["Age_Group"] = patient["Current_Age"].apply(assign_age_group)

#### 3️⃣ Computing Cascade Indicators
tx_curr = patient[patient["CurrentStatus_28"] == "Active"]

eligible = tx_curr[tx_curr["ART_start_date"].notna()]

cutoff = pd.Timestamp.today() - pd.DateOffset(years=1)
valid_vl = eligible[
    eligible["Date_of_Viral_Load"].between(cutoff, pd.Timestamp.today())
]

suppressed = valid_vl[valid_vl["Viral_load"] < 1000]

#### 4️⃣ Generating Summary Tables
summary = pd.DataFrame({
    "TX_CURR": len(tx_curr),
    "Eligible": len(eligible),
    "Valid VL Results": len(valid_vl),
    "Suppressed": len(suppressed)
})

#### 5️⃣ Visualization (Combo Chart Example)
fig, ax = plt.subplots(figsize=(10,6))

ax.bar(plot_df[group_col], plot_df["TX_CURR"])
ax.plot(plot_df[group_col], plot_df["VL Coverage (%)"], marker="o")

plt.title("VL Cascade by District")
plt.savefig("visuals/cascade_district.png", dpi=300)

### 📊 Charts & Visual Outputs

1️⃣ General VL Cascade


2️⃣ Viral Load Cascade by Partner

3️⃣ Viral Load Cascade by Age Bands

4️⃣ Viral Load Cascade by District

🔍 Key Findings (Synthetic Dataset)
⭐ General Cascade

VL coverage and suppression are low and high respectively among tested clients.

⭐ Age Bands

Adolescents show lower test coverage.

Children (<15 yrs) have fewer valid VL results.

⭐ Districts

District performance varies widely — clear opportunities for targeted supervision.

⭐ Partners

Mixed results among partners, demonstrating need for individualized technical support.

🧠 Insights

The eligibility → valid VL testing gap is the most critical program barrier.

Suppression remains high once testing occurs, highlighting treatment success.

Strengthening VL sample collection workflows would significantly improve outcomes.

Adolescents require tailored adherence and testing interventions.

✔️ Recommendations

Implement automated VL eligibility reminders.

Improve adolescent-friendly clinic services.

Strengthen facility-level sample tracking.

Increase focus on underperforming districts.

Enforce data validation at the point of entry.

🚀 Next Steps

Build an SQL + Python automated backend.

Create a live Power BI dashboard.

Integrate ML models for non-suppression prediction.

Add SMS notification logic for clients due for VL.

👤 Author

Andy – Public Health Physician • HIV Specialist • Health Data Scientist

🔗 LinkedIn Post Placeholder

Paste your LinkedIn celebration post here once published:

LinkedIn Post:
https://www.linkedin.com/posts/YOUR-POST-LINK
