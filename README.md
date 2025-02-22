# Dental Practice Database Design

**Author:** Ellie Solhjou  
**Course:** CS585-DBMS  
**Assignment:** HW1  

---

## 1️⃣ Overview
This document describes the **Entity-Relationship (ER) model** for a dental practice management system. It provides a structured approach to managing **patient care, staff scheduling, financial tracking, billing, insurance claims, and medical records**. The database is designed for **scalability, data integrity, and efficient query performance**.

---

## 2️⃣ Design Principles

### ✅ Key Functional Areas
- **Patient Management:** Tracks **appointments, medical records, and billing**.
- **Staff Management:** Handles **scheduling, salaries, and licensing**.
- **Financial Management:** Aggregates **income, expenses, and insurance claims**.
- **Operations Management:** Assigns **rooms, equipment, and services to appointments**.

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
- **Key operational insights** included in the report:
  - **Total number of patients served**
  - **New patient acquisitions**
  - **Total insurance claims processed**  
- **Insurance payments** are explicitly tracked, distinguishing between **total insurance payments** and **out-of-pocket payments**, providing a **clear breakdown of revenue sources**.

---

## 5️⃣ Bridge Entities (Many-to-Many Relationships)

To maintain **normalization and scalability**, the following **bridge entities** have been implemented to manage **Many-to-Many (M:M) relationships** effectively.

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

## 6️⃣ Scheduling & Operations Assumptions

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

## 7️⃣ Relationships & Justifications

### 📌 Business & Operations
| **Entities** | **Relationship** | **Justification** |
|-------------|-----------------|------------------|
| `BUSINESS → STAFF` | `1:M` | A **business** employs **multiple staff**. |
| `BUSINESS → BUILDING` | `1:1` | A **business** leases **one building**. |
| `BUSINESS → MONTHLY_EXPENSES` | `1:M` | A **business incurs multiple expenses**. |

### 📌 Financial & Billing
| **Entities** | **Relationship** | **Justification** |
|-------------|-----------------|------------------|
| `LOAN → MONTHLY_EXPENSES` | `1:M` | A **loan is repaid monthly** through expenses. |
| `BUILDING → MONTHLY_EXPENSES` | `1:M` | **Lease payments** are tracked in **monthly expenses**. |
| `PATIENT_BILLING_RECORD → INSURANCE_PAYMENT` | `1:M` | **A billing record may be paid by multiple insurances**. |

### 📌 Scheduling & Treatment
| **Entities** | **Relationship** | **Justification** |
|-------------|-----------------|------------------|
| `PATIENT → APPOINTMENT` | `1:M` | A **patient** can have **multiple appointments**. |
| `STAFF → LICENSE` | `1:M` | Each **staff member** must maintain **active licenses**. |
| `ROOM → APPOINTMENT` | `1:M` | **Appointments are scheduled in rooms**. |

---

## 8️⃣ Additional Assumptions

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
