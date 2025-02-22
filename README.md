# CS585-DBMS

HW1 - Ellie Solhjou

README - Dental Practice Database Design

**1. Overview**

This document outlines the design choices, assumptions, and key considerations made while constructing the Entity-Relationship (ER) Diagram for a dental practice management system. The system captures essential business operations, staff assignments, patient management, financial transactions, insurance coverage, scheduling, and reporting.

**2. Design Choices**

**a. Use of Bridge Entities for Many-to-Many Relationships**

The following bridge entities have been implemented to manage many-to-many  relationships effectively:**

**SUPPLY_SERVICE_ROOM:** Manages the assignment of supplies and equipment to services in specific rooms.

**APPOINTMENT_SERVICE:** Tracks services performed in each appointment.

**INSURANCE_COVERAGE:** Links patients to their respective insurance policies.

**INSURANCE_PAYMENT:** Tracks insurance contributions to patient billing records.

**DAILY_STAFF_ASSIGNMENT:** Maintains the assignment of staff to daily schedules.

**ROOM_ASSIGNMENT:** Manages staff-room assignments for procedures.

**TREATMENT_RECORD:** Links patients, staff, and treatments.

**BILLING_SERVICE_DETAILS:** Tracks which services are included in patient billing records.

**DAILY_SERVICE_REPORT:** Associates services with end-of-day reports.

**MONTHLY_SERVICE_REPORT:** Associates services with monthly reports.

**MONTHLY_BILLING_RECORD:** Links patient billing records to monthly salary generation.

**EXPENSE_DETAILS:** Tracks supplies and equipment included in monthly expenses.

To maintain normalization and ensure scalability, we introduced bridge entities for many-to-many (M:M) relationships. Some key bridge entities include:

**SUPPLY_SERVICE_ROOM:** Manages the assignment of supplies and equipment to services in specific rooms.

**APPOINTMENT_SERVICE:** Tracks services performed in each appointment.

**INSURANCE_COVERAGE:** Links patients to their respective insurance policies.

**INSURANCE_PAYMENT:** Tracks insurance contributions to patient billing records.

**DAILY_STAFF_ASSIGNMENT:** Maintains the assignment of staff to daily schedules.

**b. One-to-One and One-to-Many Relationships**

Each patient has exactly one medical record to ensure consistency in tracking dental history.

Each patient billing record is associated with one appointment to prevent duplicate transactions.

Each room stores multiple supplies, but each supply is assigned to only one room to track resource allocation effectively.

**c. Financial Management Assumptions**<br>

Monthly salaries are funded by patient billing records (linked via the MONTHLY_BILLING_RECORD bridge entity).<br>

Each monthly report aggregates multiple end-of-day reports to maintain financial summaries per month.<br>

Each patient billing record may be paid partially or fully by multiple insurance policies or paid out-of-pocket.<br>

Loan repayment is included in financial tracking: The business has a $300,000 loan to be repaid over 10 years. Each monthlY expenses report includes the loan installment and interest as part of fixed costs.<br>

The supplies and equipment entity includes all costs related to dental operations. It covers everything from startup costs—such as furniture, dental equipment, software for scheduling and billing, supplies, and training—to recurring monthly operating costs, including facilities cleaning, utilities, food in the breakout room, and essential medical items like needles, drugs, and paper towels.<br>

The monthly report provides a high-level financial and operational summary of the dental practice to help business owners assess profitability and business performance. This report includes aggregated income, expenses, and key patient metrics to support informed decision-making.

The monthly report is linked to the business entity, ensuring that each report is 
generated specifically for a given business.

All expenses are aggregated and tracked separately in the MONTHLY_EXPENSES entity, which includes staff salaries, lease payments, loan installments, supply purchases, and utility costs. The TOTAL_EXPENSES field in the monthly report reflects the sum of all costs recorded in MONTHLY_EXPENSES for that month.

Revenue is calculated from patient billing records, which include both insurance reimbursements and direct patient payments.

Profitability is determined by calculating NET_PROFIT_LOSS = TOTAL_REVENUE - TOTAL_EXPENSES.

Key operational insights are included, such as the total number of patients served, new patients acquired, and the number of insurance claims processed.

Insurance payments are explicitly tracked, distinguishing between total insurance payments and out-of-pocket payments to provide a clear breakdown of revenue sources.

**d. Scheduling and Operations**<br>

Each schedule of the day report assigns multiple staff members, ensuring efficient workforce distribution. It lists all assigned staff, patients, and rooms.<br>
Each end-of-day report contributes to only one monthly report, preventing duplicate reporting.<br>

Each room appears in multiple daily schedules, ensuring accurate tracking of room usage.<br>

Patient scheduling states are explicitly tracked: Each patient is assigned a scheduling status, which includes:<br>

--Contacted (patient has been reached out to but not yet scheduled)<br>

--Scheduled (appointment confirmed)<br>

--Recently Visited (patient completed an appointment recently)<br>

--Up for Next Visit (patient is due for a follow-up)<br>

--Dormant (patient has not scheduled a visit for a long time)<br>

**3. Additional Assumptions**<br>

Each type of service is performed by only one type of staff (e.g., orthodontists perform orthodontic procedures, surgeons perform surgeries, hygienists perform cleanings, etc.).<br>

Staff licenses are tracked: Every medical professional (hygienists, dentists, specialists) is required to maintain an active license. The LICENSE entity tracks license issuance, expiration, and renewal dates.<br>

A building can be leased only by one business.

A dental practice operates from one building only.<br>

Each staff member works for only one dental practice at a time.<br>

Insurance coverage is optional for patients (denoted by an O-cross foot notation in the ER diagram).<br>

Each service requires specific supplies and equipment and cannot be performed without them.<br>

Each treatment is recorded individually in the patient’s medical record.<br>

**4. Naming Conventions**

Business entity referes to the dental practice.

Entity names are in singular form (e.g., PATIENT, STAFF, ROOM) for clarity.

Bridge entities follow a structured format (e.g., SUPPLY_SERVICE_ROOM, INSURANCE_COVERAGE, DAILY_STAFF_ASSIGNMENT).

All entity names and bridge entities are in uppercase for consistency in documentation.

**5. Relationships and Justifications**

Below are the relationships along with their justifications as provided:**

**BUSINESS → STAFF (1:M)** - A business employs multiple staff members, but each staff works for only one business.

**BUSINESS → BUILDING (1:1)** - A business leases only one building.

**BUSINESS → MONTHLY_EXPENSES (1:M)**- A business incurs multiple monthly expenses.

**BUSINESS → MONTHLY_REPORT  (1:M)**  - A business generates multiple monthly reports.

**BUSINESS → SCHEDULE_OF_DAY_REPORT (1:M)**- A business generates daily schedules, each belonging to one business.

**BUSINESS → END_OF_DAY_REPORT (1:M)** - A business generates multiple end-of-day reports.

**BUSINESS → SUPPLIES AND EQUIPMENT (1:M)** - A business orders multiple supplies, but each supply belongs to one business.

**BUSINESS → LOAN (1:M)** - A business can take multiple loans, but each loan is for one business.

**BUSINESS → PATIENT (1:M)** - A business serves multiple patients, but each patient is associated with one business.

**BUSINESS → INSURANCE_PROVIDER (M:M via INSURANCE)** - A business works with multiple insurance providers, and vice versa.

**BUSINESS → PATIENT_BILLING_RECORD (1:M)** - A business generates multiple patient billing records.

**BUSINESS → MONTHLY_INCOME (1:M)** - A business generates multiple streams of income.

**STAFF → LICENSE (1:M)** - Each staff holds multiple licenses, but each license belongs to one staff.

**STAFF → ROOM (M:M via ROOM_ASSIGNMENT)** - Staff are assigned to multiple rooms, and rooms host multiple staff.

**STAFF → APPOINTMENT (1:M)** - Staff schedule multiple appointments, but each appointment is scheduled by one staff.

**STAFF → SERVICE (M:1)** - Each staff performs only one service, but multiple staff may perform the same service.

**STAFF → PATIENT (M:M via TREATMENT_RECORD)** - Staff treat multiple patients, and patients are treated by multiple staff.

**STAFF → MONTHLY_SALARY (1:M)** - Each staff receives multiple monthly salaries over time.

**BANK → LOAN (1:M, Optional)** - A bank may issue multiple loans, but each loan comes from only one bank.

**LOAN → MONTHLY_EXPENSES (1:M)** - A loon appears on multiple monthly expenses since business have loan repaymengt on monthly bases, but each monthly_expenses refers to exactly one loan.

**BUILDING → MONTHLY_EXPENSES(1:M)** -  A building incurs multiple monthly lease expenses, but each lease expense is associated with only one building

**BUILDING → ROOM (1:M, Optional)** - A building might have zero or several rooms, but each room belongs exactly to only one building.

**ROOM → APPOINTMENT (1:M)** - Each room hosts multiple appointments, but each appointment takes place in only one room.

**ROOM → SERVICE (M:M via SUPPLY_SERVICE_ROOM)** - Each room is prepared for multiple services, and services can be performed in multiple rooms.

**PATIENT → APPOINTMENT (1:M)** - Each patient books multiple appointments, but each appointment is for one patient.

**PATIENT → MEDICAL_RECORD (1:1)** - Each patient has one medical record.

**PATIENT → INSURANCE (M:M via INSURANCE_COVERAGE, Optional)** - Each patient may have multiple insurances, and each insurance covers multiple patients.

**PATIENT → PATIENT_BILLING_RECORD (1:M)** - Each patient has multiple billing records, but each billing record belongs to only one patient.

**PATIENT → SERVICE (M:M via TREATMENT_RECORD)** - Each patient undergoes multiple services, and each service is performed on multiple patients.

**APPOINTMENT → SERVICE (M:M via APPOINTMENT_SERVICE)** - Each appointment includes multiple services, and each service appears in multiple appointments.

**APPOINTMENT → PATIENT_BILLING_RECORD (1:1)** - Each appointment generates one patient billing record.

**APPOINTMENT → SCHEDULE_OF_DAY_REPORT (1:M)** - Each appointment appears in one daily schedule, but each daily schedule contains multiple appointments.

**PATIENT_MEDICAL_RECORD → STAFF (M:M via TREATMENT_RECORD)** - Each patient medical record is processed by multiple staff, and each staff processes multiple patient records.

**PATIENT_BILLING_RECORD → SERVICE (M:M via BILLING_SERVICE_DETAILS)** - Each patient billing record includes multiple services, and each service appears in multiple billing records.

**PATIENT_BILLING_RECORD → INSURANCE (M:M via INSURANCE_PAYMENT, Optional)** - Each patient billing record may be paid by multiple insurances, but insurance payment is optional.

**PATIENT_BILLING_RECORD → END_OF_DAY_REPORT (1:M)** - Each billing record contributes to only one end-of-day report, but each end-of-day report consists of multiple billing records.

**INSURANCE → PATIENT (M:M via INSURANCE_COVERAGE, Optional)** - Each insurance policy may cover multiple patients, and each patient may have multiple insurance policies.

**INSURANCE_PROVIDER → INSURANCE (1:M)** - Each insurance provider offers multiple insurance policies, but each policy is issued by one provider.

**SERVICE → SUPPLIES & EQUIPMENT (M:M via SUPPLY_SERVICE_ROOM)** - Each service requires multiple supplies and equipment, and each supply/equipment is used in multiple services.

**SERVICE → END_OF_DAY_REPORT (M:M via DAILY_SERVICE_REPORT)** - Each service appears in multiple end-of-day reports, and each end-of-day report includes multiple services.

**SERVICE → MONTHLY_REPORT (M:M via MONTHLY_SERVICE_REPORT)** - Each service contributes to multiple monthly reports, and each monthly report includes multiple services.

**MONTHLY_SALARY → PATIENT_BILLING_RECORD (M:M via MONTHLY_BILLING_RECORD)** - Each monthly salary is funded by multiple patient billing records, and each billing record contributes to multiple salaries.

**MONTHLY_SALARY → MONTHLY_REPORT (1:M)** - Each monthly salary appears in one monthly report, but each monthly report contains multiple salaries.

**MONTHLY_EXPENSES → SUPPLIES & EQUIPMENT (M:M via EXPENSE_DETAILS)** - Each monthly expense includes multiple supplies and equipment, and each supply/equipment may appear in multiple expenses.

**MONTHLY_EXPENSES → MONTHLY_REPORT (1:1)** - Each monthly expenses record appears in only one monthly report. Each monthly report contributes to one monthly expenses report.

**SUPPLIES & EQUIPMENT → SERVICE (M:M via SUPPLY_SERVICE_ROOM)** - Each supply is used in multiple services, and each service requires multiple supplies.

**ROOM → SUPPLIES & EQUIPMENT (1:M)** - Each room stores multiple supplies and equipment, but each supply is stored in only one room.

**SCHEDULE_OF_DAY_REPORT → STAFF (M:M via DAILY_STAFF_ASSIGNMENT)** - Each schedule assigns multiple staff for work, and each staff appears in multiple schedules.

**MONTHLY_REPORT → END_OF_DAY_REPORT  (1:M)** - Each end-of-day report contributes to only one monthly report, but each monthly report consists of multiple end-of-day reports.





