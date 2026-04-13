# Development of a Functional Agricultural Services Database
**Course Project: Year 2 Semester 2** **Group 12**

## 📌 Project Overview
This project involves the design and implementation of **AgriculturalSupportDB**, a relational database system developed for the Ministry of Agriculture, Animal Industry and Fisheries (MAAIF). The system solves the challenge of tracking coffee production, input distribution, and agricultural extension services across Uganda.

### The Team (Group 12)
* Kobumanzi Trishia 
* Kamagara Albert 
* Owino Esther Lynn 
* Tonny Blair Daniel 
* Onyang Bridget 

---

## ⚙️ System Capabilities
The database facilitates a tripartite ecosystem involving **Farmers**, **Extension Workers**, and **Ministry Staff**:
* **Farmer Management:** Registration of farmers and farms, including GPS coordinates and coffee varieties (Robusta/Arabica).
* **Production Tracking:** Seasonal logging of harvest data with a verification workflow.
* **Extension Services:** Logging of farm visits, advisory notes, and follow-up requirements.
* **Supply Chain:** Tracking the distribution of seedlings, fertilizers, and tools from central inventory to specific farms.

---

## 🛠 Database Design & Architecture

### Key Features (EERD)
* **Generalization & Specialization:** Utilizes a `Users` parent entity with disjoint child entities (`Farmer`, `ExtensionWorker`, `MinistryStaff`).
* **Weak Entities:** `ProductionRecords` (dependent on Farm/Season) and `FarmVisits` (dependent on Farm/Worker).
* **Data Integrity:** Implemented via Foreign Keys with `ON DELETE CASCADE` for geographical hierarchies (District → Sub-County → Village).

### Technical Constraints & Assumptions
1.  **Role Exclusivity:** A user can only hold one primary role.
2.  **Location Accuracy:** Every farm must have GPS coordinates and belong to a specific administrative village.
3.  **Workflow Validation:** A farm must have a logged visit before becoming eligible for product distribution.
4.  **Verification:** Production records remain "Pending" until verified by an assigned Extension Worker.

---

## 💻 Implementation Details

### Security & Role-Based Access Control (RBAC)
We implemented specific database roles to ensure data security:
* **Farmer:** Access to personal dashboards and production input.
* **Worker:** Permissions to update production status, log visits, and manage distributions.
* **Staff:** Full administrative privileges over the database and inventory.

### Automation & Logic
* **Triggers:** * `disjoint_user_role`: Enforces that a user cannot belong to multiple roles.
    * `stock_update`: Automatically deducts inventory upon distribution.
    * `verify_production`: Validates status updates by extension workers.
* **Stored Procedures:**
    * `RegisteringSystemUser`: Atomic registration across the `Users` and role-specific tables.
    * `RecordProduction`: Streamlines the seasonal data entry for farmers.
* **Views:** Custom dashboards for Farmers (Production status), Staff (Inventory levels), and Workers (Pending tasks).

---

## 💾 Backup & Recovery
The database is backed up using `mysqldump` to ensure data persistence:
```bash
mysqldump -u root -p --routines --triggers AgriculturalSupportDB > agric_backup.sql
```

---

## 📚 References
* Government of Uganda (2021). *The National Coffee Act*.
* Uganda Coffee Development Authority (UCDA).
* Ministry of Agriculture, Animal Industry and Fisheries (MAAIF).

---
