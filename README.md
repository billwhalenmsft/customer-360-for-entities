# Customer 360 for Entities

A Customer 360 dashboard web resource for Dynamics 365 / Dataverse — embeddable on Account, Contact, and Case forms.

> **⚠️ IMPORTANT DISCLAIMER**
>
> This solution is provided **as-is** for demonstration and enablement purposes only. It is **not an official Microsoft product** and is **not supported by Microsoft Product Support Services**. By installing this solution, you acknowledge that:
>
> - This is a **community/field-developed solution** — not a 1st-party Microsoft offering
> - Any modifications, customizations, or deployments are **your organization's responsibility**
> - Microsoft makes **no warranties**, express or implied, regarding this solution's fitness for production use
> - You should **test thoroughly in a non-production environment** before deploying to production
> - **Support** for this solution is limited to the GitHub Issues tab on this repository
> - This solution may **read data from your Dataverse environment** via the Web API — review the source code before installing
>
> **USE AT YOUR OWN RISK.**

---

## What It Does

Renders a unified Customer 360 view inside the D365 form experience — a single HTML web resource that pulls live data from Dataverse and displays it in a modern dashboard layout.

### Data Displayed

| Section | Source Entity | What You See |
|---------|-------------|-------------|
| **Account Header** | `account` | Name, phone, address, website, account number |
| **Health Score** | Calculated | Composite score (0-100) based on case volume, SLA adherence, entitlement usage |
| **Customer Tier** | `account.accountcategorycode` + `entitlement` names | Tier 1 Strategic, Tier 2 Key Account, Tier 3, Tier 4 |
| **Open Cases** | `incident` | Count, priority breakdown, recent cases with ticket number + title |
| **High Priority** | `incident` (priority=1) | Count of urgent cases needing attention |
| **Active Orders** | `salesorder` | Count, order numbers, total amounts |
| **Entitlements** | `entitlement` | Remaining terms, case count balance |
| **Key Contacts** | `contact` (by parent account) | Name, title, email, phone |
| **Financial Metrics** | Simulated from account name | Revenue trend, A/R, credit utilization, invoices |
| **Inventory and Supply Chain** | Simulated from account name | Stock levels, fill rate, lead times |

> **Note:** Financial, inventory, and supply chain metrics are **simulated** (deterministic seed from account name) for demo purposes. Replace the rendering functions with real API calls to connect to your ERP.

### Entity Detection

The web resource auto-detects where it is embedded:

| Form Type | Behavior |
|-----------|----------|
| **Account form** | Shows that account's full 360 |
| **Case (incident) form** | Resolves the case's customer account then shows that account's 360 |
| **Contact form** | *(Coming in v4)* Will detect contact type and show End Customer or Channel Contact view |

---

## What Is In The Box

```
customer-360-for-entities/
  README.md
  src/
    bw_Customer360_v3.html          <- The web resource source (85KB single-file HTML)
  solution/
    README.md
    Customer360forEntities_1_0_0_0.zip   <- Unmanaged D365 solution
```

### Solution Contents

| Component | Type | Name |
|-----------|------|------|
| Web Resource | HTML | `bw_Customer360_v3` (Display: "Customer 360 v3") |

**That is it.** One web resource. No custom entities, no plugins, no flows, no dependencies.

---

## Prerequisites

| Requirement | Details |
|-------------|---------|
| **Dynamics 365** | Customer Service, Sales, or any model-driven app with Dataverse |
| **Entities used** | `account`, `contact`, `incident`, `salesorder`, `entitlement` |
| **Permissions** | The logged-in user must have read access to the entities above |
| **Browser** | Edge, Chrome, Firefox (modern — uses ES6, CSS Grid, Flexbox) |

### What You DO NOT Need

- No Field Service required
- No Marketing required
- No Power BI required
- No additional licenses beyond your existing D365 license
- No Azure resources
- No plugins or custom code to deploy separately

---

## Installation

### Option A: Import via Power Apps Portal

1. Download `solution/Customer360forEntities_1_0_0_0.zip` from this repo
2. Go to [make.powerapps.com](https://make.powerapps.com) then Solutions then Import
3. Upload the ZIP then click Next then Import
4. Wait for import to complete (about 30 seconds)

### Option B: PAC CLI

```bash
# Authenticate to your environment
pac auth create --url https://your-org.crm.dynamics.com

# Import the solution
pac solution import --path solution/Customer360forEntities_1_0_0_0.zip --publish-changes
```

### After Import: Add to Your Forms

1. Open [make.powerapps.com](https://make.powerapps.com) then Tables then Account then Forms
2. Edit the Main form (e.g., "Account for Interactive Experience" or your custom form)
3. Add a new Tab and name it `C360` or `Customer 360`
4. Inside the tab, add a 1-column section
5. In that section, add a Web Resource control
6. Select `bw_Customer360_v3` from the web resource picker
7. Set properties:
   - Width: Full
   - Height: 600px (or more — it is responsive)
   - Scrolling: Auto
   - Border: No
8. Save then Publish

Repeat for the Case form if you want C360 visible when agents open a case.

---

## How It Works (Technical)

- Single self-contained HTML file — no external JS libraries, no CDN dependencies
- Uses Fluent UI icons (inline SVG sprites — no network calls)
- Reads data via the Dataverse Web API (`/api/data/v9.2/`) using the form's authentication context
- Detects the form entity via `Xrm.Page` / `getFormContext()`
- All rendering is client-side vanilla JavaScript — no React, no Angular, no build step
- Financial metrics use a deterministic hash of the account name to generate consistent demo data

---

## Customization

### Replace Simulated Financial Data

The following functions generate simulated data and can be replaced with real API calls:

- `renderOverviewFinancials(accountName)` — call your ERP API
- `renderInventory(accountName)` — call your inventory system
- `renderSupplyChain(accountName)` — call your supply chain API
- `renderARMetrics(accountName)` — call your A/R system
- `renderInvoices(accountName)` — call your invoicing API
- `renderPaymentHistory(accountName)` — call your payment system
- `renderRevenueByCategory(accountName)` — call your revenue analytics
- `renderRevenueTrend(accountName)` — call your revenue trends

### Change the Health Score Algorithm

Find `calculateHealthScore()` in the source and adjust the weights. Current weights are approximately: open cases volume 30%, high priority ratio 25%, entitlement usage 25%, and order activity 20%.

---

## Roadmap

- Contact-level 360 — detect Contact forms and render End Customer (household) or Channel Contact (distributor employee) views
- Customer type detection — use `accountcategorycode` / `customertypecode` for layout switching
- Configurable sections — admin toggle for which panels to show/hide
- Real ERP connectors — sample integrations for SAP, D365 Finance and Operations, NetSuite

---

## Contributing

Issues and pull requests welcome. If you have connected real financial data or added new sections, we would love to see it.

---

## License

MIT

---

> *Built by the Discrete Manufacturing Agentic CoE team. Not an official Microsoft product. Not supported by Microsoft Product Support Services. Use at your own risk.*
