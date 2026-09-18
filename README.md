# HEALTHCARE DATA ANALYSIS USING SQL
Patient Demographics, Medical Encounters, Insurance Costs & Procedures Analysis

Massachusetts General Hospital — Synthetic Patient Dataset (2011–2022)

## Table of Contents
- [Project Overview](https://github.com/OluwaseunOkundalaye/Hospital-Records-Analysis-with-SQL#project-overview)
- [Dataset Overview](https://github.com/OluwaseunOkundalaye/Hospital-Records-Analysis-with-SQL#dataset-overview)
- Database Structure & Relationships
- Data Quality Assessment
- Data Cleaning & Error Correction
- Feature Engineering
- Patient Demographic Analysis
- Medical Encounter Analysis
- Insurance & Financial Analysis
- Medical Procedure Analysis
- Key Findings & Insights
- Recommendations
- Conclusion

## PROJECT OVERVIEW
**Project Background**

This project analyzes a synthetic healthcare dataset of approximately 1,000 Massachusetts General Hospital patients from 2011–2022, covering demographics, medical encounters, insurance, and procedures.

**Project Purpose**

To use SQL to assess healthcare data quality and generate insights into patient demographics, healthcare utilization, medical costs, insurance coverage, and procedures.

**Project Objectives**
- Establish relationships between database tables.
- Perform data quality checks and corrections.
- Engineer useful analytical features.
- Analyze patient demographics and healthcare utilization.
- Analyze healthcare costs and insurance coverage.
- Analyze medical procedures and readmission patterns.

**Business Questions**
- Who are the patients being served?
- How do healthcare encounters change over time?
- What are the most common encounter types and diagnoses?
- Which payers account for the highest healthcare costs?
- How much of healthcare costs are covered by insurance?
- What are the major out-of-pocket cost patterns?
- What are the most common and costly procedures?
- What patterns of readmission exist?

**Project Scope**

The analysis covers four tables: Patients, Encounters, Payers, and Procedures, using SQL for database structuring, data quality assessment, feature engineering, and healthcare analysis.

## DATASET OVERVIEW
**Dataset Description**

The dataset is a synthetic healthcare dataset containing approximately 1,000 patient records and their healthcare activities, including demographics, medical encounters, insurance coverage, and procedures.

**Data Source and Coverage**

The dataset represents patients and healthcare activities at Massachusetts General Hospital covering the period 2011–2022.

[Click here to download the dataset](https://drive.google.com/file/d/1TaPaExDmi9fJkxIn8uT7aeQ2MZdrLuoD/view?usp=sharing)

| Name | Score | Grade | Remark |
|-----|-----|-----|------|
|Seun | 40 | A | Good |
| Favour | 50 | B | Good |
| Kelly | 66 | C | Very Good |

| Table | Field | Description |
|---|---|---|
| encounters |  | Patient encounter data |
| encounters | Id | Primary Key. Unique Identifier of the encounter. |
| encounters | Start | The date and time (iso8601 UTC Date (yyyy-MM-dd'T'HH:mm'Z')) the encounter started |
| encounters | Stop | The date and time (iso8601 UTC Date (yyyy-MM-dd'T'HH:mm'Z')) the encounter concluded |
| encounters | Patient | Foreign key to the Patient. |
| encounters | Organization | Foreign key to the Organization. |
| encounters | Payer | Foreign key to the Payer. |
| encounters | EncounterClass | The class of the encounter, such as ambulatory, emergency, inpatient, wellness, or urgentcare |
| encounters | Code | Encounter code from SNOMED-CT |
| encounters | Description | Description of the type of encounter. |
| encounters | Base_Encounter_Cost | The base cost of the encounter, not including any line item costs related to medications, immunizations, procedures, or other services. |
| encounters | Total_Claim_Cost | The total cost of the encounter, including all line items. |
| encounters | Payer_Coverage | The amount of cost covered by the Payer. |
| encounters | ReasonCode | Diagnosis code from SNOMED-CT, only if this encounter targeted a specific condition. |
| encounters | ReasonDescription | Description of the reason code. |
| patients |  | Patient demographic data. |
| patients | Id | Primary Key. Unique Identifier of the patient. |
| patients | BirthDate | The date (YYYY-MM-DD) the patient was born. |
| patients | DeathDate | The date (YYYY-MM-DD) the patient died. |
| patients | Prefix | Name prefix, such as Mr., Mrs., Dr., etc. |
| patients | First | First name of the patient. |
| patients | Middle | Middle name of the patient. |
| patients | Last | Last or surname of the patient. |
| patients | Suffix | Name suffix, such as PhD, MD, JD, etc. |
| patients | Maiden | Maiden name of the patient. |
| patients | Marital | Marital Status. M is married, S is single. Currently no support for divorce (D) or widowing (W). |
| patients | Race | Description of the patient's primary race. |
| patients | Ethnicity | Description of the patient's primary ethnicity. |
| patients | Gender | Gender. M is male, F is female. |
| patients | BirthPlace | Name of the town where the patient was born. |
| patients | Address | Patient's street address without commas or newlines. |
| patients | City | Patient's address city. |
| patients | State | Patient's address state. |
| patients | County | Patient's address county. |
| patients | FIPS County Code | Patient's FIPS county code. |
| patients | Zip | Patient's zip code. |
| patients | Lat | Latitude of Patient's address. |
| patients | Lon | Longitude of Patient's address. |
| payers |  | Insurance payer data. |
| payers | Id | Primary key of the Payer (e.g. Insurance). |
| payers | Name | Name of the Payer. |
| payers | Address | Payer's street address without commas or newlines. |
| payers | City | Street address city. |
| payers | State_Headquartered | Street address state abbreviation. |
| payers | Zip | Street address zip or postal code. |
| payers | Phone | Payer's phone number. |
| procedures |  | Patient procedure data including surgeries. |
| procedures | Start | The date and time (iso8601 UTC Date (yyyy-MM-dd'T'HH:mm'Z')) the procedure was performed. |
| procedures | Stop | The date and time (iso8601 UTC Date (yyyy-MM-dd'T'HH:mm'Z')) the procedure was completed, if applicable. |
| procedures | Patient | Foreign key to the Patient. |
| procedures | Encounter | Foreign key to the Encounter where the procedure was performed. |
| procedures | Code | Procedure code from SNOMED-CT. |
| procedures | Description | Description of the procedure. |
| procedures | Base_Cost | The line item cost of the procedure. |
| procedures | ReasonCode | Diagnosis code from SNOMED-CT specifying why this procedure was performed. |
| procedures | ReasonDescription | Description of the reason code. |

```SQL
--A. Creating relationhops between the tables
--1. Encounters and Tatients Tables -- Unique ID to the patients
	--Encounter table(PATIENT) -- Foreign key
	--Patients table(id) -- Primary Key

	--Adding primary key to ID column on Patient Table
Alter Table patients
Add constraint PK_id_patients
Primary key (id)

	--Adding Foreign key to PATIENT column on encounter Table
Alter table encounters
Add Constraint FK_Patient_encounters
Foreign key (Patient) References patients (id)

--2. Encounters and payers Tables -- Unique ID to the payer 
	--Ecounters table(PAYER) -- Foreign Key
	--Payers table(id) -- Primary Key

	--Adding primary key to ID column on Payers Table
Alter Table payers
Add Constraint PK_id_payers
Primary Key (id)

	--Adding Foreign Key to PAYER column on ecounters table
Alter Table encounters
Add Constraint FK_Payer_encounter
Foreign Key (Payer) References Payers (id)

--3. Encounter and Procedures Table -- Unique ID to the encounter
	--Encounter table(id) -- Primary Key
	--Procedure table(Encounter) - Foreign Key

	--Adding Primary Key to ID column on Encounter Table
Alter Table Encounters
Add Constraint PK_ID_Encounters
Primary Key (id)
	
	--Adding Foreign Key to Encounter Column on Procedure Table
Alter Table Procedures
Add Constraint FK_Encounter_Procedures
Foreign Key (Encounter) References Encounters (id) 

--4. Pateints and Procedure Table -- Unique ID to the Patient
	--Pateints Table(ID) -- Primary Key
	--Procedures Table(Patient) -- Foreign Key

	--Adding Primary Key to ID column on Patients Table
Alter Table Patients
Add constraint PK_id_Patients
Primary Key (id)
--(Already did this above in No 1)

	--Adding Foreign Key to Patient column on Procedures Table
Alter Table Procedures
Add Constraint FK_Patient_Procedures
Foreign Key (Patient) References Patients (id)
```

![](https://github.com/OluwaseunOkundalaye/Hospital-Records-Analysis-with-SQL/blob/main/Dashboard.png)

```Total Profit = SUMX('sales','sales'[Units] * RELATED('Product'[Profit]))```Y

```PYTHON
bins = [0, 2, 5, 10, 100]

labels = [
    'New Customer',
    'Established Customer',
    'Long-Term Customer',
    'Very Long-Term Customer'
]

df['tenure_group'] = pd.cut(
    df['customer_tenure_years'],
    bins=bins,
    labels=labels,
    include_lowest=True
)
```
