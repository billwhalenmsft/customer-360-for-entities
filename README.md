# Customer 360 for Entities

A Customer 360 dashboard web resource for Dynamics 365 / Dataverse.

## What it does

Embeds on Account, Contact, or Case forms as a tab — shows a unified view of:
- **Cases** — open/closed count, priority breakdown, recent cases list
- **Orders** — active orders, total value, line items
- **Entitlements** — remaining terms, SLA tracking, usage
- **Contacts** — key contacts with role, phone, email
- **Financial Metrics** — revenue trend, A/R, credit utilization (simulated for demo)
- **Health Score** — composite score based on case volume, SLA adherence, entitlement usage

## Screenshots

*(Coming soon)*

## Installation

### Option A: Import the solution
1. Download `solution/Customer360forEntities_1_0_0_0.zip`
2. Go to **make.powerapps.com** → Solutions → Import
3. Upload the ZIP → Import
4. Add `bw_Customer360_v3` web resource to your Account form as an IFRAME tab

### Option B: PAC CLI
```bash
pac auth create --url https://your-org.crm.dynamics.com
pac solution import --path solution/Customer360forEntities_1_0_0_0.zip --activate-plugins --publish-changes
```

## Configuration

### Add to Account Form
1. Open the Account main form in the form designer
2. Add a new **Tab** → name it "C360"
3. Add a **1-column section** inside the tab
4. Add a **Web Resource** control → select `bw_Customer360_v3`
5. Set the web resource to **full width** and height ~600px
6. Save + Publish the form

### Add to Case Form
Same steps — the web resource auto-detects the entity type:
- On an **Account** form → shows the account's full 360
- On a **Case** form → resolves the case's customer account → shows that account's 360

## Dependencies

**None.** This solution contains only a web resource. It reads data via the Dataverse Web API at runtime, so it works with whatever entities exist in the target environment.

## Roadmap

- [ ] **Contact-level 360** — detect if the form is a Contact (not Account) and render an End Customer or Channel Contact view
- [ ] **Customer type detection** — use `accountcategorycode` or `customertypecode` to render different layouts for distributors vs end consumers
- [ ] **Configurable sections** — hide/show sections based on which Dynamics apps are installed

## License

MIT
