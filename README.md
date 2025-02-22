# Dental Practice Database Design

**Author:** Ellie Solhjou  
**Course:** CS585-DBMS  
**Assignment:** HW1  

---

## 1️⃣ Overview
This document describes the **Entity-Relationship (ER) model** for a dental practice management system. It provides a structured approach to managing **patient care, staff scheduling, financial tracking, billing, insurance claims, and medical records**. 
---

## 2️⃣ Design Principles## 

The following bridge entities have been implemented to manage many-to-many  relationships effectively:**

### ✅ Insurance & Billing
- **INSURANCE_COVERAGE**: Links patients to their respective insurance policies.  
- **INSURANCE_PAYMENT**: Tracks insurance contributions to patient billing records.  
- **BILLING_SERVICE_DETAILS**: Tracks which services are included in patient billing records.  

### ✅ Scheduling & Operations
- **DAILY_STAFF_ASSIGNMENT**: Tracks staff assigned to daily schedules.  
- **ROOM_ASSIGNMENT**: Tracks staff-room assignments for procedures.  
- **APPOINTMENT_SERVICE**: Tracks services performed during appointments.  
- **TREATMENT_RECORD**: Links patients, staff, and treatments.  

### ✅ Supplies & Equipment
- **SUPPLY_SERVICE_ROOM**: Tracks the assignment of supplies and equipment to services in specific rooms.  
- **EXPENSE_DETAILS**: Tracks supplies and equipment included in monthly expenses.  

### ✅ Reports & Financial Tracking
- **DAILY_SERVICE_REPORT**: Associates services with end-of-day reports.  
- **MONTHLY_SERVICE_REPORT**: Associates services with monthly reports.  
- **MONTHLY_BILLING_RECORD**: Links billing records to salary generation.  

---


## 3️⃣ Database Schema & ER Diagram

### 📌 ER Diagram  
_(Include an image of the ER diagram here or provide a link.)_

---

## 4️⃣ Financial & Operational Assumptions

- **Monthly salaries** are funded by **patient billing records**, which are linked via the `MONTHLY_BILLING_RECORD` bridge entity.  
- Each **monthly report** aggregates multiple **end-of-day reports** to provide **financial summaries**.  
- **Each patient billing record** may be paid **partially or fully** by **multiple insurance policies** or **out-of-pocket**.  
- **Loan repayment** is included in financial tracking. The business has a **$300,000 loan** repaid over **10 years**, and each `MONTHLY_EXPENSES` report includes **loan installments and interest** as part of fixed costs.  
- The `SUPPLIES & EQUIPMENT` entity includes all **costs related to dental operations**, covering:
  - **Startup costs**: furniture, dental equipment, scheduling & billing software, supplies, and training.  
  - **Recurring monthly costs**: cleaning services, utilities, staff food, medical supplies (needles, drugs, paper towels, etc.).  
- The **monthly report** provides a **high-level financial and operational summary** of the dental practice, allowing business owners to assess **profitability and performance**.  
- **All expenses** are **aggregated in `MONTHLY_EXPENSES`**, which includes **staff salaries, lease payments, loan installments, supply purchases, and utility costs**. The `TOTAL_EXPENSES` field in the monthly report represents the **sum of all costs recorded in `MONTHLY_EXPENSES`** for that month.  
- **Revenue** is calculated from **patient billing records**, including both **insurance reimbursements** and **direct patient payments**.  
- **Profitability** is determined as:  
  `NET_PROFIT_LOSS = TOTAL_REVENUE - TOTAL_EXPENSES`  
- **Key operational insights** included in the monthly report:
  - **Total number of patients served**
  - **New patient acquisitions**
  - **Total insurance claims processed**  
- **Insurance payments** are explicitly tracked, distinguishing between **total insurance payments** and **out-of-pocket payments**, providing a **clear breakdown of revenue sources**.

---


## 5️⃣ Scheduling & Operations Assumptions

- Each **schedule of the day report** assigns **multiple staff members** and **lists assigned staff, patients, and rooms**.  
- Each **end-of-day report** contributes to **only one monthly report**, preventing duplicate reporting.  
- Each **room appears in multiple daily schedules**, ensuring accurate tracking of room usage.  
- **Patient scheduling states are explicitly tracked**:
  - **Contacted**: Patient has been reached but not yet scheduled.  
  - **Scheduled**: Appointment confirmed.  
  - **Recently Visited**: Patient completed an appointment recently.  
  - **Up for Next Visit**: Patient is due for a follow-up.  
  - **Dormant**: Patient has not scheduled a visit in a long time.  

---

## 6️⃣ Relationships & Justifications

| **Entities** | **Relationship** | **Justification** |
|-------------|-----------------|------------------|
| `BUSINESS → STAFF` | `1:M` | A business employs multiple staff members, but each staff works for only one business. |
| `BUSINESS → BUILDING` | `1:1` | A business leases only one building. |
| `BUSINESS → MONTHLY_EXPENSES` | `1:M` | A business incurs multiple monthly expenses. |
| `BUSINESS → MONTHLY_REPORT` | `1:M` | A business generates multiple monthly reports. |
| `BUSINESS → SCHEDULE_OF_DAY_REPORT` | `1:M` | A business generates daily schedules, each belonging to one business. |
| `BUSINESS → END_OF_DAY_REPORT` | `1:M` | A business generates multiple end-of-day reports. |
| `BUSINESS → SUPPLIES & EQUIPMENT` | `1:M` | A business orders multiple supplies, but each supply belongs to one business. |
| `BUSINESS → LOAN` | `1:M` | A business can take multiple loans, but each loan is for one business. |
| `BUSINESS → PATIENT` | `1:M` | A business serves multiple patients, but each patient is associated with one business. |
| `BUSINESS → INSURANCE_PROVIDER` | `M:M via INSURANCE` | A business works with multiple insurance providers, and vice versa. |
| `BUSINESS → PATIENT_BILLING_RECORD` | `1:M` | A business generates multiple patient billing records. |
| `BUSINESS → MONTHLY_INCOME` | `1:M` | A business generates multiple streams of income. |
| `STAFF → LICENSE` | `1:M` | Each staff holds multiple licenses, but each license belongs to one staff. |
| `STAFF → ROOM` | `M:M via ROOM_ASSIGNMENT` | Staff are assigned to multiple rooms, and rooms host multiple staff. |
| `STAFF → APPOINTMENT` | `1:M` | Staff schedule multiple appointments, but each appointment is scheduled by one staff. |
| `STAFF → SERVICE` | `M:1` | Each staff performs only one service, but multiple staff may perform the same service. |
| `STAFF → PATIENT` | `M:M via TREATMENT_RECORD` | Staff treat multiple patients, and patients are treated by multiple staff. |
| `STAFF → MONTHLY_SALARY` | `1:M` | Each staff receives multiple monthly salaries over time. |
| `BANK → LOAN` | `1:M, Optional` | A bank may issue multiple loans, but each loan comes from only one bank. |
| `LOAN → MONTHLY_EXPENSES` | `1:M` | A loan appears on multiple monthly expenses since businesses have loan repayments on a monthly basis, but each `MONTHLY_EXPENSES` refers to exactly one loan. |
| `BUILDING → MONTHLY_EXPENSES` | `1:M` | A building incurs multiple monthly lease expenses, but each lease expense is associated with only one building. |
| `BUILDING → ROOM` | `1:M, Optional` | A building might have zero or several rooms, but each room belongs exactly to only one building. |
| `ROOM → APPOINTMENT` | `1:M` | Each room hosts multiple appointments, but each appointment takes place in only one room. |
| `ROOM → SERVICE` | `M:M via SUPPLY_SERVICE_ROOM` | Each room is prepared for multiple services, and services can be performed in multiple rooms. |
| `PATIENT → APPOINTMENT` | `1:M` | Each patient books multiple appointments, but each appointment is for one patient. |
| `PATIENT → MEDICAL_RECORD` | `1:1` | Each patient has one medical record. |
| `PATIENT → INSURANCE` | `M:M via INSURANCE_COVERAGE, Optional` | Each patient may have multiple insurances, and each insurance covers multiple patients. |
| `PATIENT → PATIENT_BILLING_RECORD` | `1:M` | Each patient has multiple billing records, but each billing record belongs to only one patient. |
| `PATIENT → SERVICE` | `M:M via TREATMENT_RECORD` | Each patient undergoes multiple services, and each service is performed on multiple patients. |
| `APPOINTMENT → SERVICE` | `M:M via APPOINTMENT_SERVICE` | Each appointment includes multiple services, and each service appears in multiple appointments. |
| `APPOINTMENT → PATIENT_BILLING_RECORD` | `1:1` | Each appointment generates one patient billing record. |
| `APPOINTMENT → SCHEDULE_OF_DAY_REPORT` | `1:M` | Each appointment appears in one daily schedule, but each daily schedule contains multiple appointments. |
| `PATIENT_MEDICAL_RECORD → STAFF` | `M:M via TREATMENT_RECORD` | Each patient medical record is processed by multiple staff, and each staff processes multiple patient records. |
| `PATIENT_BILLING_RECORD → SERVICE` | `M:M via BILLING_SERVICE_DETAILS` | Each patient billing record includes multiple services, and each service appears in multiple billing records. |
| `PATIENT_BILLING_RECORD → INSURANCE` | `M:M via INSURANCE_PAYMENT, Optional` | Each patient billing record may be paid by multiple insurances, but insurance payment is optional. |
| `PATIENT_BILLING_RECORD → END_OF_DAY_REPORT` | `1:M` | Each billing record contributes to only one end-of-day report, but each end-of-day report consists of multiple billing records. |
| `INSURANCE → PATIENT` | `M:M via INSURANCE_COVERAGE, Optional` | Each insurance policy may cover multiple patients, and each patient may have multiple insurance policies. |
| `INSURANCE_PROVIDER → INSURANCE` | `1:M` | Each insurance provider offers multiple insurance policies, but each policy is issued by one provider. |
| `SERVICE → SUPPLIES & EQUIPMENT` | `M:M via SUPPLY_SERVICE_ROOM` | Each service requires multiple supplies and equipment, and each supply/equipment is used in multiple services. |
| `SERVICE → END_OF_DAY_REPORT` | `M:M via DAILY_SERVICE_REPORT` | Each service appears in multiple end-of-day reports, and each end-of-day report includes multiple services. |
| `SERVICE → MONTHLY_REPORT` | `M:M via MONTHLY_SERVICE_REPORT` | Each service contributes to multiple monthly reports, and each monthly report includes multiple services. |
| `MONTHLY_SALARY → PATIENT_BILLING_RECORD` | `M:M via MONTHLY_BILLING_RECORD` | Each monthly salary is funded by multiple patient billing records, and each billing record contributes to multiple salaries. |
| `MONTHLY_SALARY → MONTHLY_REPORT` | `1:M` | Each monthly salary appears in one monthly report, but each monthly report contains multiple salaries. |
| `MONTHLY_EXPENSES → SUPPLIES & EQUIPMENT` | `M:M via EXPENSE_DETAILS` | Each monthly expense includes multiple supplies and equipment, and each supply/equipment may appear in multiple expenses. |
| `MONTHLY_EXPENSES → MONTHLY_REPORT` | `1:1` | Each monthly expenses record appears in only one monthly report. Each monthly report contributes to one monthly expenses report. |
| `MONTHLY_REPORT → END_OF_DAY_REPORT` | `1:M` | Each end-of-day report contributes to only one monthly report, but each monthly report consists of multiple end-of-day reports. |

---

## 7️⃣ Additional Assumptions

- Each **type of service** is performed by only **one type of staff** (e.g., **hygienists perform cleanings**, **orthodontists perform braces**).  
- **Staff licenses are tracked**: The `LICENSE` entity records **issuance, expiration, and renewal dates**.  
- **A building can be leased only by one business.**  
- **A dental practice operates from one building only.**  
- **Each staff member works for only one dental practice at a time.**  
- **Insurance coverage is optional for patients** (denoted by an **O-cross foot notation in the ER diagram**).  
- **Each service requires specific supplies and equipment** and **cannot be performed without them**.  
- **Each treatment is recorded individually** in the `PATIENT_MEDICAL_RECORD` entity.  

---

### **📌 Notes**
- The README is **formatted correctly for GitHub, VS Code, and Markdown-compatible editors**.
- Each section follows **best practices for database documentation**.
- **Tables and lists** improve readability and organization.
