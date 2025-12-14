# 🌉 Hybrid Identity Architecture: Active Directory to Entra ID Synchronization

&nbsp;

## 🚀 Executive Summary
For most enterprise organizations (Financial Services, Healthcare, Gov), "Cloud-Only" is not yet reality. They operate in a **Hybrid State**, managing legacy on-premise servers alongside modern cloud tenants.

In this project, I engineered a **Hybrid Identity Solution** to bridge this gap. I deployed a Windows Server 2022 Domain Controller to simulate an on-premise datacenter, automated user provisioning via PowerShell, and implemented **Microsoft Entra Connect** to synchronize identities to the cloud. This architecture enables a "Single Identity" model, allowing users to access cloud resources using their on-premise credentials.

---

&nbsp;

## ⚡ Key Technologies
* **Hypervisor:** VMware Workstation Pro / Windows Server 2022
* **Directory Services:** Active Directory Domain Services (AD DS)
* **Automation:** PowerShell (Active Directory Module)
* **Identity Bridge:** Microsoft Entra Connect (Sync Engine)
* **Cloud Tenant:** Microsoft Entra ID 

---

&nbsp;

## 📸 Implementation Documentation

### Phase 1: On-Premise Infrastructure Build
I provisioned a new **Active Directory Forest** (`ad.nylabs.com`) to serve as the authoritative identity source. This involved promoting a Windows Server 2022 VM to a Primary Domain Controller (PDC).

<img width="741" height="225" alt="Domain-config-1" src="https://github.com/user-attachments/assets/393361f7-0f7b-41e8-baa7-bffef2129f98" />

*Figure 1: Initializing the new Forest Root Domain to establish the local identity authority.*


&nbsp;

After promotion, I verified the server's role within the `Domain Controllers` Organizational Unit (OU) to confirm the directory service was active and healthy.


<img width="683" height="166" alt="Primary Domain Controller -2" src="https://github.com/user-attachments/assets/f44fc576-d14b-4d3a-8d5a-fc37950cb8cf" />

*Figure 2: Verifying the PDC status within Active Directory Users and Computers (ADUC).*

---

&nbsp;


### Phase 2: Automated User Provisioning
To simulate a realistic enterprise environment, I avoided manual entry. I developed a **PowerShell script** to batch-create users with standardized attributes (Department, Job Title) directly into a specific OU (`NYLabs Users`).


<img width="837" height="641" alt="PowerShell Script Output-3" src="https://github.com/user-attachments/assets/38f6da27-1c02-49ec-a867-649e3521c747" />

*Figure 3: Executing the automation script to populate the directory with test identities.*

---

&nbsp;


### Phase 3: The "Identity Bridge" (Entra Connect)
I installed and configured **Microsoft Entra Connect** to link the on-premise forest with the cloud tenant. I configured the sync engine to aggregate users from the local `ad.nylabs.com` domain and project them into Entra ID.

<img width="875" height="622" alt="Ready to Configure-entraconnect-sync-4" src="https://github.com/user-attachments/assets/a27be8a6-93cf-402a-a123-c8de7325820f" />

*Figure 4: Configuring the Sync Engine to handle Password Hash Synchronization (PHS) and object attribute mapping.*

---

&nbsp;


### Phase 4: Validation & Synchronization
The final validation confirmed that the "Old World" users (Leddy, PJ, etc.) were successfully synchronized to the "New World" (Entra ID). The **"On-premises sync enabled"** attribute confirms the hybrid connection is live.


<img width="732" height="544" alt="On-premises sync-users-5" src="https://github.com/user-attachments/assets/db981375-823f-446d-94e9-d8db1fe3a109" />

*Figure 5:Verifying that on-premise users are now present in the Cloud Tenant with Sync enabled.*

---
## Phase 5: Advanced Configuration (Self-Service Password Reset)
**Objective:** Enabled Password Writeback to allow users to reset their passwords in the Cloud (Entra ID) and have the changes synchronize back to the On-Premise Domain Controller. This ensures users can recover their accounts even if they are working remotely without a VPN.

### Configuration Steps
* **Permission Hardening:** Manually granted **"Reset Password"** and **"Write lockoutTime"** permissions to the `ADSyncMSA` service account on the domain root via Active Directory Users and Computers (ADUC).
* **Sync Engine Config:** Re-configured the Entra Connect Sync engine to enable the **Password Writeback** feature.

### Verification
**1. Sync Engine Configuration:**
Entra Connect Writeback Config:<img width="829" height="343" alt="Screenshot 2025-12-12 211450" src="https://github.com/user-attachments/assets/4b1904be-48d6-448f-ba20-5e9619248f27" />

Figure 6:Successfully configured the sync engine without permission errors.


&nbsp;


**2. Cloud Integration Status:**
SSPR Enabled in Cloud:<img width="811" height="444" alt="Screenshot 2025-12-12 212715" src="https://github.com/user-attachments/assets/91958be1-297a-4ecd-849f-d423b99cfd31" />

Figure 7:Verified that the On-Premises Integration status is active in the Entra Admin Center, confirming the two-way sync bridge is operational.*

&nbsp;


## 🏆 Project Outcome
* **Unified Identity:** Achieved a single identity plane where changes in Active Directory (on-prem) automatically reflect in Microsoft 365 (cloud).
* **Operational Efficiency:** Reduced administrative overhead by scripting bulk user creation rather than using manual GUI entry.
* **Enterprise Readiness:** Demonstrated the exact core competency required for Identity Engineer roles at hybrid organizations.
