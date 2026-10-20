# 21 CFR Part 11 Electronic Records Audit Trail Logger

## 📌 Project Context & Overview
This repository houses the advanced regulatory compliance architecture and data schema files for a **SCADA Supervisory Electronic Records Audit Trail Engine**.

In pharmaceutical and bio-manufacturing facilities, data integrity breaches during mid-batch operations introduce heavy validation risks. To enforce strict alignment with **21 CFR Part 11 compliance criteria** and **GAMP 5 data integrity guidelines**, this project establishes a non-IT security architecture. It ensures that no Critical Process Parameter (CPP) can be altered without capturing a tamper-evident electronic footprint mapping **who, what, when, and why** a parameter modification loop occurred.

---

## 🏗️ Regulatory Control Flow & Privilege Infrastructure
The data security framework introduces a secure validation layer between operator workstation entry panels and the physical plant floor PAC architectures (Purdue Model Level 2 Optimization):

```text
 [ Station Operator HMI ]              [ Secure Audit Engine ]                [ GxP Secure Database ]
            │                                     │                                      │
            ├─────── 1. Request Change ──────────►│                                      │
            │                                     ├────── 2. Hash User Signature ───────►│
            │                                     │                                      │ (Logs Transaction Record)
            │◄────── 3. Prompt Password ──────────┤                                      │
            │                                     ├────── 4. Verify & Commit Record ────►│ (Unalterable File Write)
            ▼                                     ▼                                      ▼
```

### Critical Security Provisions:
* **Electronic Signature Interlocks:** Modifying high-priority tags (such as dosing speeds or system manifold vacuum pressures) blocks automated track execution routines until two-factor signature validation is completed on the terminal.
* **Immutable Change Rationale:** The system records raw pre-modification states and post-modification data stamps, binding them directly to an encrypted database table with mandatory engineering change rationale indices.

---

## 📁 Repository Directory Structure

```text
📁 pharma-audit-logger
├── 📁 docs
│   ├── 📝 Project6_21CFR_Audit_Trail_Specification.md         <- User privilege matrices & system path rules
│   └── 📊 Project6_Unalterable_Database_Register_Schema.csv  <- Cryptographic hashes, data columns, & links
└── 📄 README.md                                              <- Core system brief & portfolio documentation index
```

---

## 📑 Core Documentation Breakdown

### 1. 21 CFR Part 11 Compliance Specification
Located at [`docs/Project6_21CFR_Audit_Trail_Specification.md`](./docs/Project6_21CFR_Audit_Trail_Specification.md), this layout document establishes the security privilege hierarchy, electronic change interception sequences, and critical process parameter authorization tables.

### 2. Unalterable Data Schema Register
Located at [`docs/Project6_Unalterable_Database_Register_Schema.csv`](./docs/Project6_Unalterable_Database_Register_Schema.csv), this engineering table itemizes immutable data types, capturing column field frameworks, SHA-256 validation criteria mappings, and hardwired loop synchronization links.

