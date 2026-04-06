# 🔁 PLM Data Migration & Testing Simulation

> Simulated migration of product lifecycle data between systems using Python, SQL validation logic, and structured test case execution — aligned with SDLC, UAT, and system validation best practices.

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python)
![Pandas](https://img.shields.io/badge/Pandas-2.0-green?logo=pandas)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter)
![Excel](https://img.shields.io/badge/Report-Excel-217346?logo=microsoft-excel)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Test Cases](https://img.shields.io/badge/Test%20Cases-31%20Passed-success)

---

## 📌 Project Overview

This project simulates a real-world **PLM (Product Lifecycle Management) data migration** scenario. It covers the full migration testing lifecycle — from source data extraction and validation, through structured test case execution, defect logging, and executive reporting.

The simulation handles four core PLM data entities: **Parts**, **BOM (Bill of Materials)**, **Documents**, and **ECO/ECR (Engineering Change Orders)**. Intentional data quality issues are injected into the source data to demonstrate the validation engine's ability to detect and report them — mirroring real migration scenarios.

---

## 🎯 Project Objectives

- Simulate extraction of PLM product data from a legacy source system
- Perform SQL-equivalent validation checks for data integrity and business rules
- Design and execute structured test cases aligned with SDLC and UAT processes
- Detect and log defects with severity classification (Critical / High / Medium)
- Generate a professional migration testing report in Excel
- Demonstrate understanding of referential integrity, orphan records, and null handling

---

## 📁 Project Structure

```
PLM_Migration_Project/
│
├── PLM_Migration_Simulation.ipynb   # Main Jupyter Notebook (end-to-end simulation)
├── PLM_Migration_Report.xlsx        # 5-sheet Excel report (executive deliverable)
├── README.md                        # Project documentation
│
├── source_parts.csv                 # 200 IT parts with injected data issues
├── source_bom.csv                   # 150 BOM entries with orphan/zero-qty issues
├── source_documents.csv             # 100 documents with null and orphan refs
├── source_eco.csv                   # 80 ECOs with business rule violations
│
├── validation_issues.csv            # 18 auto-detected defects with severity
└── test_results.csv                 # 31 test case results (PASS/FAIL log)
```

---

## 🗂️ PLM Entities & Dataset

| Entity | Records | Description |
|--------|---------|-------------|
| **Parts** | 200 | Product parts with status, revision, cost, weight, owner |
| **BOM** | 150 | Parent-child part relationships with quantity and effectivity |
| **Documents** | 100 | Drawings, specs, manuals linked to parts |
| **ECO / ECR** | 80 | Engineering change orders with priority and approval workflow |

### Intentional Data Issues Injected

| Entity | Issue | Type |
|--------|-------|------|
| Parts | Null Part Name | Null Value |
| Parts | Negative Unit Cost | Invalid Value |
| Parts | Null Revision | Null Value |
| Parts | Invalid Status value | Invalid Reference |
| Parts | Null Weight | Null Value |
| BOM | Zero Quantity | Invalid Value |
| BOM | Null Parent Part ID | Null Value |
| BOM | Non-existent Child Part | Orphan Record |
| Documents | Null Doc Name | Null Value |
| Documents | Invalid Part reference | Orphan Record |
| ECO | Null Priority | Null Value |
| ECO | Invalid Status value | Invalid Reference |
| ECO | Approved with no Approver | Business Rule Violation |

---

## 🔍 Validation Checks Performed

### Data Integrity
- Null checks on mandatory fields (Part Name, Revision, BOM IDs, Doc Name, ECO Priority)
- Duplicate record detection across all entities
- Schema completeness verification (column presence)

### Referential Integrity (SQL-equivalent)
- BOM Parent Part ID → Parts master lookup
- BOM Child Part ID → Parts master lookup
- Document Related Part ID → Parts master lookup
- ECO Affected Part ID → Parts master lookup

### Business Rule Validation
- Part status must be one of: `Released`, `In Review`, `Obsolete`, `Draft`
- ECO status must be one of: `Open`, `In Review`, `Approved`, `Rejected`, `Closed`
- ECO Priority must not be null
- Any ECO with status `Approved` must have an `Approved_By` value
- BOM Quantity must be greater than zero

---

## 🧪 Test Case Execution Summary

| Category | Test Cases | Result |
|----------|-----------|--------|
| Extraction | 8 | ✅ All Passed |
| Data Integrity | 6 | ✅ All Passed |
| Validation | 9 | ✅ All Passed |
| Migration QA | 4 | ✅ All Passed |
| Regression | 4 | ✅ All Passed |
| **Total** | **31** | **✅ 100% Pass Rate** |

Test cases are aligned with:
- **SDLC phases** — Extraction → Validation → QA → Regression
- **UAT principles** — Business rule verification, data completeness, referential integrity
- **Defect severity classification** — Critical / High / Medium

---

## 📊 Excel Report — Sheet Overview

| Sheet | Contents |
|-------|----------|
| **Executive Summary** | KPI dashboard, migration status by entity, issues breakdown |
| **Validation Issues Log** | All 18 defects with entity, field, type, severity, and timestamp |
| **Test Case Results** | All 31 TCs with expected vs actual result, PASS/FAIL, remarks |
| **Source Data Preview** | First 30 parts rows — color coded by migration status |
| **SQL Scripts Reference** | 15 SQL queries for extraction, validation, and post-migration QA |

---

## 🗄️ SQL Scripts Included

```sql
-- Find null Part Names
SELECT Part_ID FROM source_parts WHERE Part_Name IS NULL;

-- Find negative unit costs
SELECT Part_ID, Unit_Cost_INR FROM source_parts WHERE Unit_Cost_INR < 0;

-- Find orphan BOM child references
SELECT b.BOM_ID, b.Child_Part_ID
FROM source_bom b
LEFT JOIN source_parts p ON b.Child_Part_ID = p.Part_ID
WHERE p.Part_ID IS NULL;

-- Find Approved ECOs without approver (Business Rule)
SELECT ECO_ID FROM source_eco
WHERE Status = 'Approved' AND Approved_By IS NULL;

-- Post-migration record parity check
SELECT
  (SELECT COUNT(*) FROM source_parts) AS source_count,
  (SELECT COUNT(*) FROM target_parts) AS target_count;
```

> 📄 Full list of 15 SQL scripts available in the `SQL Scripts Reference` sheet of the Excel report.

---

## 🔁 Migration Lifecycle Covered

```
Source PLM System
       │
       ▼
[1] Data Extraction ──────── Record counts, schema check, source tagging
       │
       ▼
[2] Data Validation ──────── Null checks, invalid values, orphan refs, business rules
       │
       ▼
[3] Defect Logging ───────── 18 issues logged with severity & entity
       │
       ▼
[4] Test Case Execution ──── 31 TCs executed across SDLC phases
       │
       ▼
[5] Migration QA ─────────── Completion rates, failure analysis
       │
       ▼
[6] Reporting ────────────── Excel report + defect log + recommendations
       │
       ▼
Target PLM System (Post-migration validation pending go-live)
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Python 3.10+ | Core scripting and validation logic |
| Pandas | Data loading, profiling, and transformation |
| NumPy | Data generation and numerical checks |
| OpenPyXL | Excel report generation with formatting |
| Jupyter Notebook | Interactive simulation environment |
| SQL (simulated) | Validation query logic in Python |
| Excel | Executive reporting and defect tracking |

---

## 🚀 How to Run

**Step 1 — Clone or download the project**
```bash
git clone https://github.com/yourusername/PLM-Migration-Testing.git
cd PLM-Migration-Testing
```

**Step 2 — Install dependencies**
```bash
pip install jupyter pandas numpy openpyxl
```

**Step 3 — Launch the notebook**
```bash
jupyter notebook PLM_Migration_Simulation.ipynb
```

**Step 4 — Run all cells**

Go to `Kernel → Restart & Run All`

> ⚠️ Ensure all CSV files are in the same folder as the notebook before running.

---

## 💡 Key Findings & Recommendations

Based on the simulation results:

1. **Resolve Critical issues first** — 6 Approved ECOs have no `Approved_By` value; these violate business rules and must be fixed before go-live
2. **Cleanse null values** — Part Names, Revisions, and ECO Priorities must be mandatory fields enforced at source
3. **Fix orphan BOM records** — Child Part IDs in BOM must exist in the Parts master; referential integrity check is essential pre-migration
4. **Run post-migration parity SQL** — After migration, execute count-check queries to confirm no records were lost
5. **UAT sign-off required** — All High and Critical severity issues must be resolved and re-tested before final UAT approval

---

## 👩‍💻 Author

**Mausamkumari Thakur**
- 🎓 B.Tech in Information Technology — MIT ADT University
- 💼 Aspiring Data Analyst / QA Engineer | Python · SQL · PLM · Excel
- 🔗 [LinkedIn](https://www.linkedin.com/in/mausamkumari-thakur-81471b291/)
- 💻 [GitHub](https://github.com/Mausamkumarithakur)

---


---


