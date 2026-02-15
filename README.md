# Azure Hybrid Identity Lab
A hands on laboratory demonstrating the synchronization of an On-Premise Active Directory environment with Microsoft Entra ID (Azure AD) using Azure AD Connect.


# ☁️ Azure Hybrid Identity Lab: On-Prem to Cloud Sync

## 🎯 Objective
This project demonstrates the implementation of a **Hybrid Identity** infrastructure by synchronizing an on-premises Active Directory environment with **Microsoft Entra ID (formerly Azure AD)**. This lab simulates a corporate migration to the cloud, allowing for centralized management of users and resources across both local and cloud environments.

## 🔗 The Evolution of the Lab
This project is a direct sequel to my **[On-Premise Active Directory Home Lab](https://github.com/techboulnp-gif/Active-Directory-Home-Lab)**. 
* **The Foundation:** In the previous lab, I built a Windows Server 2022 Domain Controller and automated the creation of **5,000+ enterprise user accounts** using PowerShell.
* **The Goal:** In this phase, I am bridging those 5,000 local identities to the cloud using **Azure AD Connect**, enabling features like Single Sign-On (SSO) and Microsoft 365 integration.

# ☁️ Enterprise Hybrid Identity Migration (17,000+ Users)

## 🎯 Project Objective
The goal of this project was to establish a **Hybrid Identity** infrastructure by bridging an on-premises Windows Server 2022 environment with **Microsoft Entra ID (Azure AD)**. I successfully migrated and synchronized over **17,500 user identities** while implementing enterprise-grade security and filtering practices.

---

## 🛠️ Technologies & Tools Used
* **Windows Server 2022**: Host for the primary Domain Controller.
* **Active Directory Domain Services (AD DS)**: Source of local identity data.
* **Microsoft Entra Connect**: The specialized sync engine used to bridge the environments.
* **Microsoft Entra ID**: The cloud-based target for the identity migration.
* **Password Hash Synchronization (PHS)**: Used to allow seamless sign-in with local credentials.

---

# ☁️ Enterprise Hybrid Identity Migration (17,000+ Users)

## 🎯 Project Objective
The goal of this project was to architect and implement a **Hybrid Identity** infrastructure, bridging a legacy on-premises Windows Server 2022 environment with **Microsoft Entra ID (Azure AD)**. The primary mission was the bulk migration of **17,555 user identities** while maintaining strict security protocols and optimizing synchronization performance.

---

## 🛠️ Technical Stack & Infrastructure
* **Directory Services:** Active Directory Domain Services (AD DS) on Windows Server 2022.
* **Cloud Platform:** Microsoft Entra ID (Free Tier Tenant).
* **Synchronization Engine:** Microsoft Entra Connect (V2).
* **Authentication Method:** Password Hash Synchronization (PHS) with seamless sign-on capabilities.

---

## 🚀 Phase 1: On-Premises Preparation & Architecture

### 1. Identity Assessment and OU Optimization
Before initiating the migration, I conducted a deep audit of the local `mylab.local` forest. To ensure a professional migration, I isolated the target identities into a structured Organizational Unit (OU). This prevented system-level accounts and built-in administrative groups from being inadvertently synced to the cloud.

* **Inventory Audit:** Verified **17,255 distinct user objects** within the local database.
* **Namespace Isolation:** Centralized all verified identities within the **`_EMPLOYEES`** OU for targeted management.

![Local AD Source](https://raw.githubusercontent.com/techboulnp-gif/Azure-Hybrid-Identity-Lab/main/Phase%201/01_OnPrem_AD_Source.png)

### 2. Microsoft Entra Connect Configuration
I architected the hybrid bridge using the **Microsoft Entra Connect** wizard with a custom configuration focus. 

* **Source Anchor Selection:** Implemented **mS-DS-ConsistencyGuid** as the primary immutable ID to allow for future forest migrations and identity persistence.
* **UPN Mapping:** Configured the **userPrincipalName** attribute as the primary login identifier to maintain consistency between local and cloud login experiences.
* **Domain Linkage:** Established a secure trust between the `mylab.local` forest and the `techboulnp.onmicrosoft.com` tenant.

![Directory Connection](https://raw.githubusercontent.com/techboulnp-gif/Azure-Hybrid-Identity-Lab/main/Phase%201/02_Directory_Connection.png)

### 3. Implementation of Granular OU Filtering
To maximize synchronization efficiency and adhere to the principle of least privilege, I bypassed the "Sync All" default setting. I manually mapped the synchronization scope strictly to the **`_EMPLOYEES`** OU. This ensures that only authorized personnel identities are provisioned in the cloud environment.

![OU Filtering Strategy](https://raw.githubusercontent.com/techboulnp-gif/Azure-Hybrid-Identity-Lab/main/Phase%201/03_OU_Filtering_Strategy.png)

---

## ☁️ Phase 2: Cloud Provisioning & Data Integrity Verification

### 1. Synchronization Health Dashboard
Post-implementation, I utilized the **Microsoft Entra Admin Center** to monitor the "Initial Full Sync" cycle. I verified the heartbeat of the connection to ensure real-time delta syncs were functional.

* **Operational Status:** Dashboard verified as **"Enabled"** with a green health status ✅.
* **Sync Latency:** Confirmed a healthy sync interval of less than 60 minutes.

![Entra Connect Enabled](https://raw.githubusercontent.com/techboulnp-gif/Azure-Hybrid-Identity-Lab/main/Phase%202/01_Entra_Connect_Enabled.png)

### 2. Large-Scale Identity Provisioning
The synchronization successfully provisioned **17,555 cloud-native identities**. I performed a spot-check of various user profiles to ensure the **"On-premises sync enabled"** attribute was correctly inherited as **"Yes"**, signifying a perfect data handshake between environments.

![Final User Count](https://raw.githubusercontent.com/techboulnp-gif/Azure-Hybrid-Identity-Lab/main/Phase%202/02_Final_User_Count.png)

### 3. Authentication Flow Validation (PHS Test)
I validated the **Password Hash Synchronization (PHS)** by attempting a sign-on for user `bab.gasavo` via the Azure Portal. 
* **The Result:** The system successfully authenticated the credentials against the synced hash.
* **The Outcome:** Encountered an expected `AADSTS50011` redirect error, which confirms that while the **Identity and Password** were correct, the user simply lacked administrative permission to the portal—this served as an additional verification of tenant security restrictions.

---

## 🛠️ Troubleshooting & Technical Hurdles

### 🚧 Challenge 1: Sync Admin Permission Restrictions
* **Issue:** The initial setup failed because the logged-in administrator was a "Guest/External" account in the Entra Tenant, which is blocked by Microsoft security policies.
* **Solution:** I provisioned a dedicated **Cloud-Native Global Administrator** account (`syncadmin@...onmicrosoft.com`) to manage the sync agent, which is a production best practice.

### 🚧 Challenge 2: UPN Suffix Mismatch
* **Issue:** The local domain used `.local`, while the cloud required a routable suffix. 
* **Solution:** I utilized the Entra Connect wizard to override the UPN suffix warning, ensuring users could still sign in using their unique identifiers despite the non-routable local domain.

### 🚧 Challenge 3: Management of High Object Counts
* **Issue:** Local ADUC interface limited visual display to 2,000 objects, making verification difficult.
* **Solution:** Used PowerShell and Entra ID portal metrics to verify the final count of **17,555**, ensuring no objects were dropped during the migration pipeline.


## 🗺️ Project Roadmap & Portfolio Navigation

### 🚩 Current Phase: Hybrid Cloud Integration (Complete)
This project serves as the bridge between on-premises engineering and cloud identity management.

* **Step 1: On-Premise Active Directory Home Lab (Project 1)** — Establishing the local DC and 17k users. ✅
* **Step 2: Azure Hybrid Identity Lab (You are here)** — Synchronizing local AD with Entra ID via Entra Connect. ✅
* **Step 3: Advanced Cloud Management (Planned)** — Implementing RBAC, Conditional Access, and Intune Device Management. 🔜

### 🎓 Portfolio Navigation
* [ ⬅️ Previous Project: Windows Server AD Foundation ](https://github.com/techboulnp-gif/Active-Directory-Lab)
* [ ➡️ Next Project: RBAC & Cloud Governance (In Progress) ](#)

---

---
