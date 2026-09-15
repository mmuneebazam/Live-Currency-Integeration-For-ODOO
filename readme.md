# Odoo Live Currency Rate Integration
Automatically fetch, map, and synchronize real-time exchange rates in Odoo via external REST APIs. This module extends Odoo’s native currency management by adding smart duplicate-prevention logic, on-demand manual sync capabilities, and fault-tolerant error handling.
---
## Key Features
* **Automated REST API Fetching:** Uses Python `requests` to fetch live JSON exchange rate data and maps them directly to active Odoo currencies.
* **Smart Duplicate Prevention:** Extends `res.currency.rate` logic to check existing entries, ensuring clean database history without duplicate daily records.
* **On-Demand UI Sync:** Adds an explicit sync button directly on the currency form, allowing users to instantly fetch and update rates for individual currencies.
* **Fault-Tolerant Execution:** Includes robust error handling to gracefully capture API timeouts or connection drops without interrupting core Odoo operations.
---
## Architecture Overview
┌─────────────────┐ JSON Response ┌──────────────────────┐ │ External REST ├──────────────────────────►│ Odoo Python Backend │ │ Currency API │ │ (requests module) │ └─────────────────┘ └──────────┬───────────┘ │ Map & Validate Logic │ ▼ ┌─────────────────┐ Sync Trigger (Button) ┌──────────────────────┐ │ Odoo Currency ├──────────────────────────►│ res.currency.rate │ │ Form (UI) │ │ (Database) │ └─────────────────┘ └──────────────────────┘
---
## Installation & Setup
### Prerequisites
* Odoo 15.0+ (Community or Enterprise)
* Python `requests` library installed in your Odoo environment:
 ```bash
 pip install requests
Installation Steps
Clone or copy this repository into your Odoo custom addons directory:git clone [https://github.com/your-username/odoo-currency-sync.git](https://github.com/your-username/odoo-currency-sync.git)

Restart your Odoo service:sudo systemctl restart odoo

Enable Developer Mode in Odoo.
Go to Apps -> Click Update Apps List.
Search for Currency REST API Sync and click Install.
Technical Details & Code Structure
1. Model Extension (res.currency)
Core logic for triggering REST requests using requests.get().
Parses incoming JSON structures and isolates exchange rates based on target ISO codes.
2. Smart Logic Extension (res.currency.rate)
Overrides standard creation/write methods.
Validates whether a record for (currency_id, name) already exists for the current date before inserting a new entry.
3. UI View Extension (views/currency_views.xml)
Extends view_currency_form to embed a header/action button:<button name="action_sync_live_rate"
       string="Sync Live Rate"
       type="object"
       class="oe_highlight"/>

Usage
Navigate to Settings -> Accounting (or General Settings) -> Currencies.
Select any active currency (e.g., EUR, PKR, AED).
Click the Sync Live Rate button on the top form bar.
The system will make an external API call, fetch the exchange rate for today's date, and update or create the corresponding entry in the res.currency.rate table.
License
Distributed under the LGPL-3 License. See LICENSE for more information.
