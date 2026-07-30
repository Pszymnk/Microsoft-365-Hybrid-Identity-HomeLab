# 🚀 Microsoft 365 Hybrid Identity HomeLab

## 📌 Project Overview
This project demonstrates the setup, configuration, and management of a **Hybrid Identity environment** connecting an **On-Premises Windows Server 2022 Active Directory Domain Services (AD DS)** with **Microsoft Entra ID (Cloud)** using **Microsoft Entra Connect Sync**.

The goal of this lab is to simulate a real-world enterprise infrastructure transition, showcasing identity lifecycle management, attribute synchronization, and hybrid directory scoping.

---

## 🏗️ Architecture & Topology
* **On-Premises Domain Controller:** Windows Server 2022 (`lab.local`)
* **Identity Synchronization Tool:** Microsoft Entra Connect Sync
* **Authentication Method:** Password Hash Synchronization (PHS)
* **Cloud Tenant:** Microsoft 365 Business Premium (`HomeLab98.onmicrosoft.com`)
* **Scope Control:** Organizational Unit (OU) Filtering

---

## ⚙️ Key Implementation Steps & Evidence

### 1. Directory Synchronization Setup
Deployed **Microsoft Entra Connect** on the Windows Server 2022 Domain Controller and configured **Password Hash Synchronization (PHS)** to enable seamless user authentication using local AD credentials.

> **Evidence: Synchronization Service Manager confirming successful Delta/Initial Sync**
<img width="1021" height="728" alt="obraz" src="https://github.com/user-attachments/assets/7a31b5e1-a2e8-421c-89e3-249ce272146a" />

### 2. OU Filtering (Best Practice Implementation)
Instead of synchronizing the entire Active Directory forest, synchronization was restricted strictly to a dedicated **Organizational Unit (OU)**. This prevents privileged local accounts from syncing to the cloud and avoids polluting the tenant with built-in system objects.

> **Evidence: Microsoft Entra Connect domain and OU filtering configuration showing scope restricted to specific OU**
<img width="875" height="620" alt="obraz" src="https://github.com/user-attachments/assets/fa57fa52-dfd0-4408-803e-69bba26d99cc" />


### 3. Identity Lifecycle & Cloud Provisioning
Created test users in local Active Directory (acting as the **Source of Authority**), triggered synchronization, and verified their appearance in the cloud tenant.

> **Evidence: Entra ID Portal showing synced users (On-premises sync enabled: Yes)**
<img width="1919" height="471" alt="obraz" src="https://github.com/user-attachments/assets/a178a92f-57ca-4049-b4ea-e78d0cd0ddfb" />


### 4. Attribute Flow Validation (Source of Authority)
Updated user organizational attributes (e.g., `Job Title`) in On-Premises AD and verified attribute propagation through Entra Connect to Entra ID and Exchange Online.

> **Evidence: On-Premises AD Attribute vs. Cloud Profile Card**
<img width="969" height="294" alt="obraz" src="https://github.com/user-attachments/assets/feb50d87-794b-4cd1-b6a7-9749358506d2" />


---

## ⌨️ PowerShell Operations

To force an immediate delta synchronization between On-Premises AD and Entra ID:

```powershell
Start-ADSyncSyncCycle -PolicyType Delta

