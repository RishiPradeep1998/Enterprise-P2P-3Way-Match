# Enterprise P2P Reconciliation and 3-Way Match Automation

Designed and developed an unattended Procure-to-Pay (P2P) RPA pipeline using UiPath Studio and the REFramework. 

The core of this project is a rigorous 3-Way Match engine. In corporate finance, reconciling supplier invoices against internal Purchase Orders (PO) and physical Goods Receipt Notes (GRN) requires strict, multi-layered validation. I built this workflow to handle that exact complex auditing process. It independently extracts data from invoices, cross-references it against master datasets, and ensures transactions are only authorized when all three sources align perfectly.

## Technical Architecture
**Decoupled Architecture:** Implemented a Dispatcher/Performer pattern to separate document ingestion from transaction execution, utilizing Orchestrator Queues for scalable workload management.
* **Dispatcher (`1_Dispatcher_Invoice_Ingestion`):** Scans directories, extracts relevant invoice metadata, and loads transaction items into UiPath Orchestrator Queues.
* **Performer (`2_Performer_3Way_Match`):** Built on the REFramework to consume queue items and execute the sequential validation logic.

<img width="1916" height="860" alt="Decoupled Architecture" src="https://github.com/user-attachments/assets/2ad929bf-8d9c-4b6c-baea-6f16a12a9694" />

## Sequential Fail-Fast Business Logic
Designed the validation logic to drop invalid transactions at the earliest stage to save compute time. The bot executes the following sequence:
1. **Vendor Validation:** Verifies the vendor has an active status in the master database.
2. **PO Authorization:** Confirms the Purchase Order is authorized and matches the invoiced entity.
3. **Quantity Match:** Cross-references invoiced quantities against physical Goods Receipt Notes (GRNs).
4. **Price Audit:** Audits unit price variances between the PO and the final invoice.
5. **Mathematical Recalculation:** Independently recalculates the final invoice math (Unit Price * Qty + Tax) to ensure no manual tampering or errors.
6. **Exception Routing:** Automatically moves duplicate or rejected PDFs into specific directories while archiving successful runs.

## Tech Stack
* **UiPath Studio** (REFramework, Unattended Automation)
* **UiPath Orchestrator** (Queues, Asset Management)
* **Data Integration & Exception Handling**

<img width="1517" height="927" alt="Tech Stack" src="https://github.com/user-attachments/assets/a46ff4cf-7a3f-463e-83a1-3f4be2b601c4" />

