# Functional Design Specification: 21 CFR Part 11 Electronic Records Audit Trail Logger

## 1. Engineering Objective
To define the automation design parameters, user privilege matrices, and tamper-evident logging blocks required to enforce **21 CFR Part 11 compliance** and **GAMP 5 data integrity regulations** within a supervisory SCADA/HMI layer. This system architecture ensures that no critical process parameter (CPP) can be altered without creating a permanent, unalterable electronic footprint tracking who, what, when, and why a change was executed.

## 2. Regulatory Electronic Data Flow Architecture
To guarantee data visibility integrity, any parameter change initiated at an HMI workstation must pass through an encrypted system logger before modifying localized PLC control registers:

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

1. **Change Interception:** The supervisory HMI intercepts any operator input targeting a Critical Control Variable (e.g., editing a fill volume threshold).
2. **Unalterable Hashing:** The system captures the existing variable footprint, binds it to a unique network timestamp, and routes it to an isolated logging buffer.
3. **Electronic Signature Validation:** The interface freezes and prompts the user for dual authentication (Unique Operator Username and Password pin tokens) along with a mandatory drop-down justification reason.
4. **Data Registry Release:** Once the entry is verified and securely written to a read-only GxP database, the gateway fires a write command down to update the PLC controller memory address.

---

## 3. Critical Process Parameter (CPP) Security Privilege Matrix

| Critical Process Parameter | Target Register Tag | Required User Clearance | Electronic Signature Prompt | Mandatory Justification Drop-down Reason |
| :--- | :--- | :--- | :--- | :--- |
| Dosing Pump Displacement Speed | CFG_Pump_RPM | Automation Engineer | Active (Dual-Factor Authentication) | Tooling Replacement / Fluid Viscosity Drift |
| Needle Manifold Stoppering Vacuum | CFG_Vac_mBar | Calibration Technician | Active (Token Pin Re-entry) | Analog Sensor Recalibration / Line Setup |
| Conveyor Indexing Infeed Timer | CFG_Index_ms | Line Supervisor | Inactive (Single User Log Entry Only) | Mechanical Optimization Run |
| Nitrogen Purge Flow Tolerance | CFG_N2_Lpm | Quality Assurance Auditor | Active (Administrator Key Override) | GxP Validation Validation Run Changes |
