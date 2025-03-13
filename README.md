<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>🏰 On-Premises Active Directory Configured & Deployed in the Cloud (Azure)</h1>

## Overview
This guide details the process of implementing an on-premises Active Directory (AD) environment within Azure Virtual Machines. Follow along to configure a fully functional AD deployment in the cloud.

---

📚 **Environments and Technologies Used**
- Active Directory Domain Services (AD DS)
- Group Policy Management
- Microsoft Azure (Virtual Machines/Compute)
- PowerShell ISE (Admin Mode)

💻 **Operating Systems Used**
- Windows Server 2022
- Windows 10 (21H2)

---

## 🔍 High-Level Deployment and Configuration Steps
- Installing Active Directory on **DC-1**
- Promoting **DC-1** to Domain Controller Status
- Creating **Organizational Units (OUs)**
- Adding an **Admin User** within the Domain
- Connecting the **Client Machine** to the Domain
- Organizing the Client Machine in Active Directory
- Implementing **Group Policy for Remote Desktop Access**
- Automating **User Creation with PowerShell**

---
## 🔢 Deployment and Configuration Steps

### 🛠 Installing Active Directory on DC-1
1. Open **Server Manager** > Click **Add roles and features**.
2. Proceed through the wizard:
   - **Server Selection**: Ensure "DC-1" is selected.
   - **Server Roles**: Check **Active Directory Domain Services**.
   - Click **Add Features** > Continue through installation.
   - Enable **Restart if required** > Click **Install**.
3. Once complete, click **Close**.

---
### 🌟 Promoting DC-1 to a Domain Controller
1. In **Server Manager**, click the **Flag Icon** (Notifications) > Select **Promote this server to a domain controller**.
2. Choose **Add a new forest** > Set the Root domain to `mydomain.com`.
3. Set a **Directory Services Restore Mode (DSRM) password**.
4. Uncheck **Create DNS delegation** > Click **Next**.
5. Proceed through installation; the machine will restart.
6. Log in using the **Domain Admin** account (e.g., `mydomain\jane_admin`).

---
### 🛠️ Creating Organizational Units (OUs)
1. In **DC-1**, open **Active Directory Users and Computers**.
2. Right-click `mydomain.com` > **New** > **Organizational Unit**.
3. Create the following OUs:
   - **_ADMINS**
   - **_EMPLOYEES**
   - **_CLIENTS**

---
### 🤵 Creating an Admin User
1. In **Active Directory Users and Computers**:
   - Right-click **_ADMINS** > **New** > **User**.
   - Name: `Jane Admin`, Username: `jane_admin`.
   - Set a password (e.g., `Password1`) and enable **Password never expires**.
2. Right-click `jane_admin` > **Add to Group** > Add **Domain Admins**.
3. Log out and sign in as `jane_admin`.

---
### 🚀 Connecting the Client Machine to the Domain
1. On the **Client VM**:
   - Right-click **Start** > **System** > **Rename this PC**.
   - Click **Change** > Select **Domain**.
   - Enter **`mydomain.com`** > Authenticate using **jane_admin**.
2. Restart the Client VM to apply changes.
3. Verify the **Client** machine appears in **Active Directory Users and Computers** under `Computers`.
4. Move `Client` to the **_CLIENTS** OU.

---
### 🔧 Enabling Remote Desktop for Non-Admins
1. On **DC-1**, open **Group Policy Management**.
2. Navigate to **_CLIENTS** > Right-click > **Create a GPO in this domain and Link it here...**.
3. Name the GPO **Allow Remote Desktop**.
4. Edit the new GPO:
   - Navigate to **Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Remote Desktop Services** > **Connections**.
   - Enable **Allow users to connect remotely**.
5. Apply the policy and verify.

---
### 💻 Automating User Creation with PowerShell
1. Open **PowerShell ISE** as Administrator.
2. Download and open the script: [Generate-Names-Create-Users.ps1](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-AD-Users.ps1).
3. Run the script to populate the **_EMPLOYEES** OU with **1,000 test users**.
4. Verify users appear in **Active Directory Users and Computers** under **_EMPLOYEES**.

---
### 🎉 Wrapping Up
Congratulations! You’ve successfully deployed and configured **Active Directory** in **Azure** with a fully functional **Domain Controller**, **Client Machine**, and **automated user creation**. This setup mirrors a real-world enterprise environment, demonstrating foundational IT administration skills.

👍 Great work! Now, go forth and **admin like a pro!**

