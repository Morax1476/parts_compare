# PartsCompare

> **A Web-Based Bill of Materials Comparison System Using Curated Supplier Offers**  
> *Capstone Project Nexus · College of Engineering and Digital Technology (A.Y. 2026–2027)*

---

## 🚀 Quick Start

PartsCompare is an internal company web application with zero external dependencies.

### Option 1: Direct File Launch
Double-click [`index.html`](./index.html) in Windows File Explorer or open it in Google Chrome, Microsoft Edge, or Mozilla Firefox.

### Option 2: Local HTTP Server (Native Windows PowerShell)
Right-click [`start-server.ps1`](./start-server.ps1) and choose **Run with PowerShell** (or run `powershell -ExecutionPolicy Bypass -File .\start-server.ps1` in your console). The application will launch at:
```text
http://localhost:8000/
```

---

## 🔐 Company Accounts & Authentication System

In accordance with company operational requirements and the Project Nexus capstone specification, **PartsCompare is an internal enterprise tool without public self-registration**. All employee accounts are provisioned and governed by the **Administrator**.

The application comes pre-seeded with authorized accounts for testing:

| Employee Name | Username | Password | Role | Responsibilities |
| :--- | :---: | :---: | :---: | :--- |
| **Christian Dave D. Guiyab** | `dave_pm` | `password123` | **Maker** | Inputs & validates BOMs, reviews offer eligibility, and saves permanent sourcing decision summaries. |
| **Pearl Alexa J. Catequista** | `pearl_dba` | `password123` | **Catalog Maintainer** | Curates exact part offers, sets prices, records stock availability, checks dates, and manages deactivations. |
| **Jeffeson C. Sadoguio** | `jeffeson_admin` | `password123` | **Administrator** | Provisions company user accounts, configures system freshness rules, and monitors the immutable audit log. |

> **Evaluator Testing:** You can also click the user profile pill in the top-right corner to instantly switch between employee sessions with 1 click during capstone defense demonstrations.

---

## 🌟 Key Features & Implementation Architecture

| Category | Capability & Enhancement |
| :--- | :--- |
| **User Authentication & RBAC** | • Company login screen with credential checks.<br>• Session persistence across tabs and browser restarts.<br>• **Employee Provisioning:** Administrator can provision new company user accounts on the fly.<br>• Role-enforced capabilities: Makers have read-only access to catalog forms; Maintainers have full catalog authoring permissions. |
| **BOM Management** | • Exact part identifier and positive integer validation (`qty > 0`).<br>• Duplicate-part atomic rejection.<br>• **Live Reactive Steppers (`+` / `−`)** to test stock coverage thresholds instantly.<br>• **CSV File Import & Export** for rapid bill-of-materials onboarding. |
| **Eligibility Engine** | • Dynamic candidate evaluation against the 8 explicit exclusion criteria.<br>• **"Inspect Candidates" Modal Dialog:** Click on any BOM line to inspect every offer in the catalog with full green/red audit criteria breakdown.<br>• Support for manual candidate selection among eligible offers. |
| **Curated Catalog** | • Attribution to source, quote date, recorded available quantity, and unit price in integer centavos (`priceMinor`).<br>• **Soft Deactivation Toggle:** Switch offers active/inactive without destructive database deletes.<br>• **Synthetic Price Drift Simulator:** Test catalog price drift with one click. |
| **Sourcing Summaries** | • **Immutable Decision Snapshots:** Copies prices, quantities, dates, candidate offers, and preparer attribution at commit time.<br>• **Catalog Drift Diff Mode:** Compare historical saved values against live catalog prices side-by-side with delta badges.<br>• **JSON Snapshot Export.** |
| **Defense & Reporting** | • **Capstone Demo Scenario Switcher:** Switch between 4 preset defense scenarios (`Baseline`, `100% Complete`, `Strict 7-Day Freshness`, `High-Volume Coverage Shortage`).<br>• **Printable Formal Sourcing Report:** College-formatted printable report sheet with maker and maintainer signature approval blocks. |
| **Auditing & Governance** | • Configurable freshness age policy (1–180 days).<br>• Persistent audit event ledger tracking employee actions. |

---

## 🧪 Scripted Verification Scenarios

### Scenario 1: Default Baseline ("Ultrasonic Sensor Node")
1. Log in as **Christian Dave Guiyab** (`dave_pm`).
2. Go to **Compare BOM** and click **Reset sample BOM**.
3. **Observed Behavior:**
   - 4 parts resolve cleanly: `ATMEGA328P-PU`, `HC-SR04`, `SG90`, `NE555P`.
   - `LM7805` is **Unresolved** (Sample Shop B only has 2 recorded, 3 needed; Sample Shop C is 41 days old).
   - `1N4007` is **Unresolved** (No offer in catalog).
   - Subtotal displays **Partial Parts Subtotal of ₱1,134.00** across **4 of 6 lines costed**. Line costs for unresolved items are strictly marked *Not counted* (stored as `null`).
4. **Interactive Check:** Click **−** on `LM7805` to lower quantity from `3` to `2`. Notice how `LM7805` **immediately turns Resolved** with Sample Shop B, and the subtotal updates in real time!

### Scenario 2: Candidate Offers Audit Inspection
- Click the **Inspect** button on the `LM7805` line.
- The inspection flyout reveals all candidate offers in the catalog:
  - Sample Shop B: Excluded due to `INSUFFICIENT_QUANTITY (2 recorded, 3 needed)`.
  - Sample Shop C: Excluded due to `STALE (checked 41 days ago; limit 14)`.

### Scenario 3: Catalog Drift Isolation & Diff Mode
1. Switch account to **Pearl Alexa Catequista** (Catalog Maintainer).
2. Go to the **Curated catalog** tab.
3. Click **Simulate catalog price drift** (raising HC-SR04 from ₱92.00 to ₱95.00).
4. Go to the **Saved summaries** tab and inspect the saved summary:
   - Notice that the saved snapshot **preserves ₱92.00** and the **₱1,134.00 subtotal**.
5. Click **Compare with current catalog**:
   - The table displays the live catalog column showing `₱95.00 (+₱3.00 drift)`, proving that decisions are permanently insulated from future catalog changes.

### Scenario 4: User Provisioning (Administrator)
1. Switch account to **Jeffeson Sadoguio** (Administrator).
2. Go to **Administration** tab.
3. In **Provision company account**, create a new account for a new team member.
4. The user is instantly registered in the authorized company users table.

---

## 📁 Repository Structure

```text
parts compare/
├── index.html            # Main single-page application (UI + Engine + Auth + Storage)
├── start-server.ps1      # Native Windows PowerShell local web server
├── README.md             # Project documentation and defense walkthrough
```

*Architectural documentation and SQL schema:*  
See [`SCHEMA_AND_ENGINE_DESIGN.md`](../.gemini/antigravity/brain/898d17b2-bae0-4e65-a758-1bd56d8831e5/SCHEMA_AND_ENGINE_DESIGN.md) for the complete database schema specification and sequence diagrams.
