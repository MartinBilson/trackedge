# Trackedge Technologies AMS
1. ✅ Project Overview
2. 🛠️ Module Features
3. 🧱 Folder & File Structure
4. 🧩 Models and Views
5. 🔁 Workflows and Functionality
6. 🔧 Setup Instructions
7. ⏱️ Cron Jobs (if any)
8. 🧪 Testing Instructions
9. 🔍 Notes & Improvements

Project folder contains multiple Odoo modules, including:

* `trackedge_base`
* `trackedge_product`
* Other dependencies like `formio`, `generic_request`, `crnd_web_diagram_plus`, `muk_web_*`, etc.

**professional documentation** for this project focused on the **Asset Management System** portion — modules with `trackedge_` naming, the custom work.

---

## 📘 Odoo Asset Management System – Documentation

### 1. 📦 Project Overview

This project implements a custom **Asset Management System** built on **Odoo 17**. It manages the lifecycle of organizational assets — from registration and categorization to approval workflows and product tracking.

---

### 2. 🧩 Included Modules

| Module Name         | Description                                                                                                          |
| ------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `trackedge_base`    | Core foundational module defining asset categories, departments, locations, and basic workflows.                     |
| `trackedge_product` | Extends Odoo’s product model to support asset-specific fields, approval processes, and status tracking.              |
| Dependencies        | Modules like `generic_request`, `formio`, and `muk_web_*` are used to support UI enhancements and backend workflows. |

---

### 3. 📁 Folder & File Structure

Typical structure for each module:

```
trackedge_base/
├── __init__.py
├── __manifest__.py
├── models/
│   └── *.py
├── views/
│   └── *.xml
├── security/
│   └── ir.model.access.csv
└── data/
    └── *.xml (e.g. default records, sequences)
```

---

### 4. 🔍 Models & Views

#### `trackedge_base.models.asset_item`

Defining the core asset data structure:

* Fields: `name`, `category_id`, `location_id`, `state`, `acquisition_date`, `value`, etc.
* States: Draft → Submitted → Approved → Rejected

#### `trackedge_product.models.product_product`

Extends product with:

* Asset-specific fields
* Rejection reasons
* Multi-stage approval process

#### Views

* Kanban, Tree, and Form views for Assets
* Dropdown to select rejection reasons
* Buttons for state transitions (approve, reject, etc.)

---

### 5. 🔁 Workflow Overview

1. **Create Asset Item**
   → Fill in category, location, acquisition date, etc.

2. **Submit for Approval**
   → Changes asset state to `submitted`

3. **Approver Reviews**
   → Can approve (state: `approved`) or reject (state: `rejected`) with reason dropdown

4. **Product Sync**
   → Approved items may sync into product catalog (`trackedge_product`)

---

### 6. ⚙️ Setup Instructions

#### Prerequisites

* Odoo 17 installed
* PostgreSQL configured
* Python 3.10+ with required packages

#### Installation Steps

```bash
# 1. Clone or unzip the modules into your custom addons path
cp -r trackedge-17.0/* /odoo/custom_addons/

# 2. Update config file to include path
addons_path = /odoo/custom_addons,/odoo/odoo-server/addons

# 3. Restart Odoo
./odoo-bin -u all -d your_database_name

# 4. Activate Developer Mode and install `trackedge_base` and `trackedge_product`
```

---

### 7. ⏱️ Cron Jobs

From your logs:

```plaintext
cron0 polling for jobs
```

This is a **default Odoo thread** that polls every minute to check scheduled actions (like email notifications or maintenance checks). It’s not a bug.

To define your own scheduled job in XML:

```xml
<record id="ir_cron_auto_asset_approval" model="ir.cron">
    <field name="name">Auto Approve Assets</field>
    <field name="model_id" ref="model_your_model"/>
    <field name="state">code</field>
    <field name="code">model.auto_approve_assets()</field>
    <field name="interval_number">1</field>
    <field name="interval_type">hours</field>
    <field name="active">True</field>
</record>
```

---

### 8. 🧪 Testing Instructions

* Go to **Inventory** or **Assets** menu
* Create new Asset
* Submit for approval
* Reject with reason
* Ensure correct view transitions and access rights

---

### 9. 💡 Suggestions for Improvement

* ✅ Add unit tests using `odoo.tests.common.TransactionCase`
* 📊 Integrating dashboards for asset distribution and status
* 🔐 Adding role-based access rules for different departments
* 🔁 Enable scheduled depreciation or maintenance workflows if possible on OCE
