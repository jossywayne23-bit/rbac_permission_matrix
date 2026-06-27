# RBAC Permission Matrix — Microsoft Entra ID

![Environment](https://img.shields.io/badge/Environment-Lab%20%2F%20Non--Production-blue)
![Platform](https://img.shields.io/badge/Platform-Microsoft%20Entra%20ID-0078D4?logo=microsoft)
![Test Cases](https://img.shields.io/badge/Test%20Cases-10%20%E2%9C%85%20PASS-brightgreen)
![Version](https://img.shields.io/badge/Version-1.0-lightgrey)

A structured Role-Based Access Control (RBAC) project mapping 8 enterprise job roles to granular Microsoft Entra ID permissions, validated through boundary testing and least-privilege enforcement.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Role Definitions](#role-definitions)
- [Permission Matrix Summary](#permission-matrix-summary)
- [Test Case Results](#test-case-results)
- [Screenshot Evidence](#screenshot-evidence)
- [Tools Used](#tools-used)
- [Repository Structure](#repository-structure)
- [Key IAM Concepts Demonstrated](#key-iam-concepts-demonstrated)

---

## Project Overview

**Problem this project solves:** Organisations commonly over-provision access, violating least-privilege principles and increasing their attack surface. This project designs and validates a permission model that maps job function to minimum required access — a critical control for any IAM programme.

**Scope:**
- Designed a permission matrix mapping **8 job roles** to **34 granular Entra ID permissions** across 7 categories
- Implemented RBAC using Microsoft Entra ID built-in roles and security groups
- Validated least-privilege boundaries via boundary testing in a live lab tenant
- Documented results with screenshot evidence per test case

**Environment:** Microsoft Entra ID Free / Developer Tenant (non-production)

---

## Role Definitions

| Role ID | Role Name | Department | Entra ID Group | Risk Level |
|---------|-----------|------------|----------------|------------|
| R01 | Global Administrator | IT / Security | GRP-GlobalAdmins | CRITICAL |
| R02 | Security Administrator | IT / Security | GRP-SecurityAdmins | HIGH |
| R03 | IT Helpdesk | IT Support | GRP-HelpDesk | MEDIUM |
| R04 | Application Developer | Engineering | GRP-Developers | MEDIUM |
| R05 | Finance Analyst | Finance | GRP-FinanceAnalysts | LOW |
| R06 | HR Manager | Human Resources | GRP-HRManagers | MEDIUM |
| R07 | Auditor / Compliance | Legal / Compliance | GRP-Auditors | LOW |
| R08 | End User | All Departments | GRP-AllUsers | LOW |

---

## Permission Matrix Summary

The full matrix workbook is included in this repository: [`RBAC_Permission_Matrix.xlsx`](./RBAC_Permission_Matrix.xlsx)

**Legend:**
| Symbol | Meaning |
|--------|---------|
| ✅ | ALLOW — Permission granted |
| ❌ | DENY — Permission explicitly denied / not assigned |
| ⚠️ | CONDITIONAL — Read-only or scoped access |

**Coverage by Role (34 permissions):**

| Role | ALLOW | DENY | CONDITIONAL | % Allowed |
|------|-------|------|-------------|-----------|
| Global Administrator | 34 | 0 | 0 | 100% |
| Security Administrator | 14 | 20 | 0 | 41% |
| Auditor / Compliance | 10 | 24 | 0 | 29% |
| IT Helpdesk | 5 | 28 | 1 | 15% |
| HR Manager | 5 | 28 | 1 | 15% |
| Application Developer | 4 | 30 | 0 | 12% |
| Finance Analyst | 4 | 30 | 0 | 12% |
| End User | 1 | 33 | 0 | 3% |

**Permission categories covered:**
- User Management (7 permissions)
- Group Management (4 permissions)
- Role Management (3 permissions)
- App Management (5 permissions)
- Security & Conditional Access (7 permissions)
- Billing & Cost (4 permissions)
- Directory (4 permissions)

---

## Test Case Results

All 10 boundary tests were executed in a live Microsoft Entra ID lab tenant.

| TC# | Role Under Test | Test Action | Expected | Result |
|-----|----------------|-------------|----------|--------|
| TC-01 | IT Helpdesk (R03) | Attempt to assign Global Admin role to a user | Access Denied | ✅ PASS |
| TC-02 | IT Helpdesk (R03) | Reset password for a standard user | Success | ✅ PASS |
| TC-03 | Application Developer (R04) | Attempt to grant admin consent to an app | Access Denied | ✅ PASS |
| TC-04 | Application Developer (R04) | Register a new app registration | Success | ✅ PASS |
| TC-05 | Finance Analyst (R05) | View Cost Management + billing invoices | Success (read-only) | ✅ PASS |
| TC-06 | Finance Analyst (R05) | Attempt to create / delete Azure resources | Access Denied | ✅ PASS |
| TC-07 | Auditor (R07) | Read sign-in logs and audit logs | Success (read-only) | ✅ PASS |
| TC-08 | Auditor (R07) | Attempt to modify a Conditional Access policy | Access Denied | ✅ PASS |
| TC-09 | End User (R08) | Edit own profile via MyAccount portal | Success | ✅ PASS |
| TC-10 | End User (R08) | Attempt to read another user's profile via Graph Explorer | Access Denied | ✅ PASS |

**Summary: 10/10 PASS — All least-privilege boundaries confirmed.**

---

## Screenshot Evidence

Screenshots for each test case are located in the [`/screenshots`](./screenshots/) folder.

| TC# | Evidence File |
|-----|--------------|
| TC-01 | `screenshots/role-assignment-denied.png` |
| TC-02 | `screenshots/password-reset-success.png` |
| TC-03 | `screenshots/admin-consent-denied.png` |
| TC-04 | `screenshots/app-registration-success.png` |
| TC-05 | `screenshots/billing-viewer.png` |
| TC-06 | `screenshots/resource-denied.png` |
| TC-07 | `screenshots/audit-log-access.png` |
| TC-08 | `screenshots/ca-edit-denied.png` |
| TC-09 | `screenshots/profile-edit.png` |
| TC-10 | `screenshots/graph-denied.png` |

> Screenshots show the Entra ID portal UI at the moment of access grant or denial, confirming the permission boundary behaves as designed.

---

## Tools Used

| Tool | Purpose |
|------|---------|
| Microsoft Entra ID Portal | Role assignment, user management, CA policies |
| Azure Cost Management | Billing Reader role validation |
| Microsoft Graph Explorer | API-level permission boundary testing |
| MyAccount Portal (myaccount.microsoft.com) | End user self-service profile testing |
| Entra ID App Registrations | Developer role scope validation |
| Entra ID Sign-in & Audit Logs | Auditor read-only access validation |

---

## Repository Structure

```
rbac_permission_matrix/
├── README.md                          # This file
├── RBAC_Permission_Matrix.xlsx        # Full permission matrix workbook
└── screenshots/
    ├── role-assignment-denied.png     # TC-01
    ├── password-reset-success.png     # TC-02
    ├── admin-consent-denied.png       # TC-03
    ├── app-registration-success.png   # TC-04
    ├── billing-viewer.png             # TC-05
    ├── resource-denied.png            # TC-06
    ├── audit-log-access.png           # TC-07
    ├── ca-edit-denied.png             # TC-08
    ├── profile-edit.png               # TC-09
    └── graph-denied.png               # TC-10
```

---

## Key IAM Concepts Demonstrated

**Least Privilege Enforcement** — Each role is scoped to the minimum permissions required for its job function. End users have 1/34 permissions; Global Admin is the only role with full access.

**Separation of Duties** — Security Administrators manage CA policies and MFA but cannot manage billing or HR data. Finance Analysts have billing read access but no identity management permissions.

**Conditional / Scoped Access** — IT Helpdesk and HR Manager roles use ⚠️ conditional permissions: Helpdesk can only modify display name and department attributes; HR can only reset passwords for direct reports and manage HR-scoped groups.

**Boundary Testing** — Rather than trusting configuration alone, each role was tested by signing in as that role's user and attempting both permitted and denied actions, confirming enforcement at the platform level.

**Entra ID Built-in Role Mapping** — Org roles map to appropriate Entra ID built-in roles (e.g., Finance Analyst → Billing Reader, Auditor → Global Reader) rather than custom roles, following Microsoft's recommended least-privilege role assignments.

---

*Project by Josiah Azimi | Created: 2025-06-23 | Environment: Lab / Non-Production*
