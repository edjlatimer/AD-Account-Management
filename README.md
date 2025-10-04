# Active Directory Account Lockout Policies & Account Management  

<p align="center">
  <img src="https://i.imgur.com/pU5A58S.png" alt="Active Directory Logo" width="300"/>
</p>  

##  Project Overview  
This project demonstrates how to **configure, test, and troubleshoot Active Directory account lockout policies** in a cloud-hosted lab. Using Azure-hosted virtual machines, I implemented a **Group Policy lockout threshold**, tested lockout behavior, and practiced account lifecycle management by enabling/disabling accounts and reviewing event logs.  

Key learning objectives:  
- Configuring **Account Lockout Thresholds** via Group Policy  
- Simulating **failed login attempts** to trigger lockouts  
- Unlocking and resetting user accounts in **Active Directory Users and Computers (ADUC)**  
- Disabling/re-enabling accounts and observing client-side impact  
- Reviewing **security logs** on both the Domain Controller and Client for auditing purposes  

---

##  Technologies Used  
- **Microsoft Azure** (Virtual Machines, Networking)  
- **Windows Server 2022** (Domain Controller `DC-1`)  
- **Windows 10 (21H2)** (Client Machine `Client-1`)  
- **Active Directory Users and Computers (ADUC)**  
- **Group Policy Management Console (GPMC)**  
- **Windows Event Viewer**  

---

##  Lab Architecture  

**Components:**  
- **DC-1 (Domain Controller):** Hosts Active Directory, enforces Group Policies 
- **Client-1 (Workstation):** Domain-joined Windows 10 VM used for testing account lockouts
- **Domain Users:** Standard accounts created in `_EMPLOYEES` OU 

---

##  Step-by-Step Implementation  


### 1. Getting Started 

- Log into `DC-1` via Remote Desktop as an admin (e.g., `mydomain.com\jane_admin`) 
- In **ADUC**, identify a Domain user account that will be used as the test subject (e.g., `dapo.dog`)

![AD domain user sub ](https://github.com/user-attachments/assets/abee7c39-4094-4324-bd57-25e0049bdb4f)

---

### 2. Configure Account Lockout Threshold via Group Policy  


- On `DC-1`, open **Group Policy Management Console**: Start → Run → `gpmc.msc`


![AD run GPMC](https://github.com/user-attachments/assets/2a09aa0e-5c95-4903-bf46-a37e2d51d720)


- Edit the **Default Domain Policy**  
- Navigate to:  
  - `Computer Configuration → Windows Settings → Security Settings → Account Policies → Account Lockout Policy`  


![AD default domain services](https://github.com/user-attachments/assets/1b3c61e8-df74-4d29-abac-e3313f63ed0f)


- Configure the following settings:  
  - **Account lockout threshold:** 5 invalid attempts  
  - **Account lockout duration:** 30 minutes  
  - **Reset account lockout counter after:** 10 minutes  


![AD edit default domain services ](https://github.com/user-attachments/assets/600b51b9-7941-4297-8037-617b898d21d0)


- Apply the policy and force an update with:  
  - On a client machine or server, open Command Prompt and type `gpupdate /force` then press     Enter

![AD gpupdate](https://github.com/user-attachments/assets/739c7da3-6e7e-4d6e-8e1b-864ceffab457)


---

### 3. Test the Lockout Policy  

  
- On `Client-1`, attempt to log in as a Domain User (e.g., `dapo.dog`) with an incorrect password **6 times**  
- Observe that the account is now locked out  

![AD sub login attempt](https://github.com/user-attachments/assets/af629eb3-5ea1-490d-a749-0d0ce8003e0f)

![AD sub login failed 6x](https://github.com/user-attachments/assets/84a93c0f-ecca-4129-aa36-d0e7b47d8ef8)


- Back on `DC-1`, open **ADUC → User Properties** → confirm the account is marked as locked
    
![AD verify sub is locked out](https://github.com/user-attachments/assets/9b4a9f42-facc-4dcb-b1c9-30e1d664e325)

 

---

### 4. Unlock and Reset Account Password  

  
- On `DC-1`, open **ADUC** → Find the same Domain User (`dapo.dog`)
- Right-click the locked Domain User Account (`dapo.dog`) → **Reset Password**  
- Check the **Unlock Account** option  
 
![AD reset sub password](https://github.com/user-attachments/assets/9cc6fa7c-165a-4640-8ab3-cbd4a095e511)

![AD password reset   Unlock](https://github.com/user-attachments/assets/d1699018-dc01-4851-a917-f3d3176353ba)


- Attempt to log into `Client-1` again with the new password → confirm successful login
 
![AD sub login success](https://github.com/user-attachments/assets/5e5cd54b-edc8-4b29-9302-35e34bb4d39e)

---

### 5. Disable and Re-enable Accounts  

  
- On `DC-1`, right-click the same user → **Disable Account**.  
- On `Client-1`, attempt to log in with the disabled account → observe the error message stating the account is disabled.  
- Re-enable the account in **ADUC**.  
- Retry login → confirm access is restored.  

📸 Screenshot Example:  
![Account Disabled](https://via.placeholder.com/600x300?text=Account+Disabled)  

---

### 6. Review Security Logs  

  
- On `DC-1`, open **Event Viewer**.  
- Navigate to: `Windows Logs → Security`.  
- Filter for **Audit Failure** and **Audit Success** events related to logons.  
- On `Client-1`, open **Event Viewer**.  
- Navigate to: `Windows Logs → Security`.  
- Observe failed login attempts and lockout messages from the client side.  

📸 Screenshot Example:  
![Event Viewer Logs](https://via.placeholder.com/600x300?text=Security+Logs)  

---

##  Outcome / Learnings  

By completing this lab, I practiced:  
- Configuring and enforcing account lockout policies via Group Policy.  
- Simulating failed logins to test security controls.  
- Unlocking, resetting, disabling, and re-enabling Active Directory user accounts.  
- Analyzing security event logs on both Domain Controllers and clients.  

This project demonstrates my ability to implement **account security policies** and handle common Active Directory support tasks such as lockouts, account resets, and security auditing.  
