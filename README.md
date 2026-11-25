# Hospital Readmission Risk Analysis | Power BI



## Project Overview

This project explores 30-day hospital readmission patterns using a
healthcare admission dataset and an interactive Microsoft Power BI
dashboard. It demonstrates an end-to-end analytics workflow using
**Power Query**, **DAX**, and Power BI visualisations.


## Business Objectives

* Measure the overall 30-day hospital readmission rate.
* Identify demographic and clinical groups with comparatively higher
observed readmission rates.
* Explore the relationship between follow-up appointment scheduling
and readmission outcomes.
* Examine previous readmission history, medication adherence, and
length of stay.
* Present KPIs and findings in an interactive dashboard to support
further investigation and decision-making.



## Dataset

* **Dataset:** `hospital\_readmission\_risk\_dataset\_2026\_v1\_18000rows`
* **Records:** 18,000
* **Original columns:** 25
* **Format:** CSV



The dataset includes demographic, admission, clinical, medication,
discharge, and readmission outcome fields. Key fields include `Age`,
`Gender`, `Admission\_Type`, `Primary\_Diagnosis\_Group`, `Length\_of\_Stay`,
`Severity\_Score`, `ICU\_Stay\_Flag`, `Previous\_Readmissions\_1Y`,
`Followup\_Appointment\_Scheduled`, `Medication\_Adherence\_Score`, and
`Readmitted\_Within\_30\_Days`.



> \*\*Data note:\*\* The dataset does not contain a patient identifier,
> hospital identifier, or event date. This is therefore an
> admission-record analysis, not a unique-patient, hospital-comparison,
> or time-trend analysis. Validate the source documentation and outcome
> definition before treating the results as real-world benchmarks.

Dataset Source and Attribution
---

\- Platform: Kaggle

\- Dataset owner: Sunil Kumar

\- Dataset: Hospital Readmission Risk Dataset (CSV)

\- Link: https://www.kaggle.com/datasets/sunil123kumar/hospital-readmission-risk-dataset-csv


Dataset source and credit belong to the respective Kaggle owner. This project uses the dataset for educational and portfolio analysis purposes. Please refer to the original Kaggle page for the dataset's description, licence, and usage conditions.



## Tools and Technologies

* **Microsoft Power BI Desktop** -- data modelling, DAX measures,
dashboard design, and interactive reporting.
* **Power Query** -- data import, type handling, data quality checks,
and derived categories.
* **DAX** -- KPI and readmission-rate calculations.
* **GitHub** -- project documentation and version control.



## Data Preparation

The CSV was imported through **Get Data → Text/CSV → Transform Data**.
The preparation workflow included:

1. Reviewing column names and headers.
2. Assigning appropriate text, whole-number, and decimal data types.
3. Checking missing values and duplicate records.
4. Reviewing binary flags such as ICU status, follow-up scheduling,
high-risk medication, and readmission outcome.
5. Reviewing numeric fields for invalid or implausible values. Negative
creatinine values should be investigated against the source data and
corrected only when a valid replacement is supported; they should
not be silently treated as valid measurements.
6. Creating derived categories for analysis.



### Derived columns

* **Age Group** -- groups records into age bands.
* **Severity Band** -- categorises severity as Low, Moderate, or High.
* **Prior Readmission Band** -- categorises history as None, One, or
Two or More.
* **ICU Stay** -- readable Yes/No labels from the ICU flag.
* **Follow-up Status** -- Scheduled/Not Scheduled labels.
* **Readmission Status** -- Readmitted/Not Readmitted labels.

Original source fields should be retained where possible so derived
categories remain traceable.



## Key Performance Indicators

KPI                                             Value shown

\---

Total Admissions                                     18,000
Total Readmissions               13,349 (approximately 13K)
30-Day Readmission Rate                              74.17%
Average Length of Stay                           10.05 days
Average Medication Adherence                         70.18%
ICU Admission Rate                                   19.76%

Values reflect the dataset and report filter context. They are not
validated clinical benchmarks.



## DAX Measures

The following core measures are used for KPI calculations. Adjust the
table name if your Power BI model uses a different name.

``` dax
Total Admissions =
COUNTROWS('hospital\_readmission\_risk')
```

``` dax
Total Readmissions =
CALCULATE(
    COUNTROWS('hospital\_readmission\_risk'),
    'hospital\_readmission\_risk'\[Readmitted\_Within\_30\_Days] = 1
)
```

``` dax
Readmission Rate (%) =
DIVIDE(
    \[Total Readmissions],
    \[Total Admissions],
    0
)
```

``` dax
ICU Admission Rate (%) =
DIVIDE(
    CALCULATE(
        COUNTROWS('hospital\_readmission\_risk'),
        'hospital\_readmission\_risk'\[ICU\_Stay\_Flag] = 1
    ),
    \[Total Admissions],
    0
)
```

``` dax
Average Length of Stay =
AVERAGE('hospital\_readmission\_risk'\[Length\_of\_Stay])
```

``` dax
Average Medication Adherence =
AVERAGE('hospital\_readmission\_risk'\[Medication\_Adherence\_Score])
```



Format rate measures as percentages. These measures respond to the
report's filter context.



## Dashboard Pages and Visualisations

### Page 1 -- Patient and Clinical Readmission Analysis

* KPI cards: Total Admissions, Total Readmissions, Readmission Rate,
and Average Length of Stay.
* 30-Day Readmission Rate by Age Group -- horizontal bar chart.
* Readmission Rate by Primary Diagnosis -- horizontal bar chart.
* Readmission Rate by Severity Band -- column chart.
* Readmission Rate by ICU Stay -- doughnut chart.
* Additional KPI cards: Average Medication Adherence and ICU Admission
Rate.
* Slicers: Age, Gender, Primary Diagnosis Group, and Admission Type.

### Page 2 -- Discharge, History, and Treatment Analysis

* Readmission Rate by Follow-up Appointment Status -- doughnut chart.
* Readmission Rate by Previous Readmissions -- column chart.
* Average Stay by Diagnosis -- line chart.
* Medication Adherence by Readmission Status -- doughnut chart.
* Slicers: Age Group, Gender, Primary Diagnosis Group, and Admission
Type.



## Key Findings

These are descriptive summaries of the dataset:

* The overall 30-day readmission rate is **74.17%**.
* Infection has the highest displayed diagnosis-category readmission
rate at **75.26%**.
* High-severity records have a higher readmission rate (**77.08%**)
than moderate (**73.90%**) and low-severity records (**71.51%**).
* ICU-related records show a higher readmission rate (**76.54%**) than
records without an ICU stay (**73.58%**).
* Records with a scheduled follow-up appointment show a lower
readmission rate (**73.35%**) than records without a scheduled
appointment (**76.06%**).
* The Two or More previous-readmissions group has the highest
displayed rate (**76.67%**) among the prior-history bands.
* Average length of stay ranges from **9.94 days** for Infection to
**10.11 days** for Diabetes.
* Average medication adherence is **70.55%** for non-readmitted
records and **70.05%** for readmitted records.



### Important interpretation note

The age-group visual represents each age group's **share of all
readmitted records**, not the readmission rate within that age group. It
should be interpreted as a distribution of readmissions by age group. To
calculate an age-specific readmission rate, divide readmitted records in
each age group by all admission records in that age group.

All comparisons are descriptive. They do not demonstrate that a factor
causes readmission or that a proposed intervention will reduce it.



## Business Recommendations

* Review discharge planning and post-discharge support for older
patients.
* Examine infection-related care pathways and discharge processes.
* Review discharge assessment and follow-up arrangements for
high-severity and ICU-related cases.
* Evaluate how follow-up appointments are scheduled and completed.
* Investigate repeated readmission patterns to understand recurring
care needs.
* Explore patient education and medication-support opportunities.

These are areas for further investigation, not proven interventions from
this analysis.



## Limitations

* The overall readmission rate of 74.17% is unusually high for many
real-world healthcare settings. Validate the dataset source,
sampling approach, and 30-day outcome definition before comparing it
with external benchmarks.
* No patient ID, hospital ID, or date field is available, limiting
patient-level, hospital-level, and time-trend analysis.
* The age chart displays the distribution of readmitted records by age
group, not the age-specific readmission rate.
* Observed differences may be influenced by other variables and do not
establish causation.
* Dataset provenance, field definitions, and clinical value ranges
should be checked before drawing clinical conclusions.
* This is an exploratory analytics project, not a clinical
decision-support or patient-risk prediction model.



## How to Use

1. Clone or download this repository.
2. Open the `.pbix` file if it is included.
3. To rebuild the report, import the source CSV into Power BI.
4. Use Power Query to review data types, quality, invalid values, and
derived categories.
5. Add the DAX measures, adjusting the table name if necessary.
6. Recreate the visuals and slicers listed above.
7. Use slicers to explore results across selected categories.



## Author

**Salvin George**  
Power BI | Data Analytics | SQL | Python

\---

*Developed for learning and portfolio purposes. Validate the dataset
documentation and results before using them for operational or clinical
decisions.*

