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
     ![image](https://github.com/user-attachments/assets/0cf7129e-306e-46e3-898b-4093279c70e7)
     
   - **Server Roles**: Check **Active Directory Domain Services**.
     ![image](https://github.com/user-attachments/assets/4af7218e-feef-4e37-bdca-299b9e612f7c)
     
   - Click **Add Features** > Continue through installation.
     ![image](https://github.com/user-attachments/assets/b5c7d4ba-1071-46bb-b902-c3924d5ec0dd)
     
   - Enable **Restart if required** > Click **Install**.
3. Once complete, click **Close**.

---
### 🌟 Promoting DC-1 to a Domain Controller
1. In **Server Manager**, click the **Flag Icon** (Notifications) > Select **Promote this server to a domain controller**.
2. Choose **Add a new forest** > Set the Root domain to `mydomain.com`.
   ![image](https://github.com/user-attachments/assets/541f1934-1fa0-4d9e-9408-71d9510934c0)
   
4. Set a **Directory Services Restore Mode (DSRM) password**.
   ![image](https://github.com/user-attachments/assets/292d7426-1abf-4d3b-ae83-a8e5ab43f0e8)
   
5. Uncheck **Create DNS delegation** > Click **Next**.
   ![image](https://github.com/user-attachments/assets/bc181f8d-d6ba-4894-a4b0-bcdac9748774)
   
6. Proceed through installation; the machine will restart.
7. Log in using the **Domain Admin** account (e.g., `mydomain\jane_admin`).
   ![image](https://github.com/user-attachments/assets/5a38f7b9-3cbe-4a6c-b7d8-986398fcd20a)

---
### 🛠️ Creating Organizational Units (OUs)
1. In **DC-1**, open **Active Directory Users and Computers**.
   ![image](https://github.com/user-attachments/assets/466bc82d-7e8d-4aa2-bd9e-649d47a4e5d8)
   
2. Right-click `mydomain.com` > **New** > **Organizational Unit**.
   ![image](https://github.com/user-attachments/assets/48b24411-e883-43b1-bde2-0ada584dec27)
   
4. Create the following OUs:
   - **_ADMINS**
     ![image](https://github.com/user-attachments/assets/34fb0f6f-9182-4f12-b7f1-0458a66518e5)
     
   - **_EMPLOYEES**
     ![image](https://github.com/user-attachments/assets/10e0a9cd-2b2f-4afe-a2ea-18f80a1c455e)
     
   - **_CLIENTS**
     ![image](https://github.com/user-attachments/assets/a585343c-4184-440e-9de7-ac0f2bb14abc)

---
### 🤵 Creating an Admin User
1. In **Active Directory Users and Computers**:
   - Right-click **_ADMINS** > **New** > **User**.
   - Name: `Jane Admin`, Username: `jane_admin`.
     ![image](https://github.com/user-attachments/assets/93a4e334-3efb-437c-a48c-da6d99f07abc)
     
   - Set a password (e.g., `Password1`) and enable **Password never expires**.
2. Right-click `jane_admin` > **Add to Group** > Add **Domain Admins**.
   ![image](https://github.com/user-attachments/assets/b46f20c6-2605-43e1-9855-4db5bebfb6bc)
   ![image](https://github.com/user-attachments/assets/b46f20c6-2605-43e1-9855-4db5bebfb6bc)
   ![image](https://github.com/user-attachments/assets/463166e3-7c74-4be3-be01-5aa3575273a8)
   ![image](https://github.com/user-attachments/assets/295ff298-2e95-4428-817f-4549921e9d60)
   
4. Log out and sign in as `jane_admin`.
   ![image](https://github.com/user-attachments/assets/3bff181c-fb8c-4b4e-afe4-993f3a74212e)

---
### 🚀 Connecting the Client Machine to the Domain
1. On the **Client VM**:
   - Right-click **Start** > **System** > **Rename this PC (advanced)**.
     ![image](https://github.com/user-attachments/assets/b297235f-4bac-4cca-92ae-3ef56eeb0f18)
     
   - Click **Change** > Select **Domain**.
     ![image](https://github.com/user-attachments/assets/48ac6f32-0363-4eec-bbda-2da6c5ef6402)
     
   - Enter **`mydomain.com`** > Authenticate using **jane_admin**.
     ![image](https://github.com/user-attachments/assets/48ac6f32-0363-4eec-bbda-2da6c5ef6402)
     
2. The Client VM will Restart itself apply changes.
3. Verify the **Client** machine appears in **Active Directory Users and Computers** under `Computers`.
   ![image](https://github.com/user-attachments/assets/bcb2ac81-6566-40ae-8b41-307a4f690114)
   
4. Move `Client` to the **_CLIENTS** OU.
   ![image](https://github.com/user-attachments/assets/bc13c3c4-44f1-4744-9c8e-6437682c4e13)

---
### 🔧 Enabling Remote Desktop for Non-Admins
1. On **DC-1**, open **Group Policy Management**.
   ![image](https://github.com/user-attachments/assets/d6a085b4-bce2-4093-925c-5b14abbd1c1a)
   ![image](https://github.com/user-attachments/assets/f5258fc9-4263-43fa-afe7-6da76d4b3f73)
   
3. Navigate to **_CLIENTS** > Right-click > **Create a GPO in this domain and Link it here...**.
   ![image](https://github.com/user-attachments/assets/e0b313e6-7466-4afd-bad4-1e4ce9fd6d8d)
   
4. Name the GPO **Allow Remote Desktop**.
   ![image](https://github.com/user-attachments/assets/605e8d85-9f81-4b08-9559-168a96ff942e)
   
5. Edit the new GPO:
   ![image](https://github.com/user-attachments/assets/d034fd01-6776-4ec3-8149-c7764f6c0f66)
   - Navigate to **Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Remote Desktop Services** > **Connections**.
   - Enable **Allow users to connect remotely**.
     ![image](https://github.com/user-attachments/assets/4ca421da-9381-430c-b9fe-528331df2e3e)
     
6. Apply the policy and verify it is fully enabled.
   ![image](https://github.com/user-attachments/assets/06e87b2c-5d1e-4f4a-a693-23d12533b401)
   ![image](https://github.com/user-attachments/assets/70316a60-2c04-4d13-a5d7-62b2cb3f7e5b)

---
### 💻 Automating User Creation with PowerShell
1. Open **PowerShell ISE** as Administrator.
   ![image](https://github.com/user-attachments/assets/ed90f68b-1f73-4cdf-8dcd-b333a1f9e905)
   
2. Head to this link: [Generate-Names-Create-Users.ps1](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-AD-Users.ps1).
   
3. Copy the RAW file.
   ![image](https://github.com/user-attachments/assets/c06d86e0-e545-4459-983b-7fe00fc6d98c)
   
6. Make sure to click **New File** in PowerShell
   ![image](https://github.com/user-attachments/assets/fae6bb06-d0f7-4398-b763-61d6d4b10d0c)
   
8. Run the script in PowerShell ISE to populate the **_EMPLOYEES** OU with **1,000 test users**.
   ![image](https://github.com/user-attachments/assets/31d4bdbc-9361-4e5c-890e-0f395fc08b03)
   
10. When you're ready **Stop the operation**
    ![image](https://github.com/user-attachments/assets/4944cd38-9c27-463a-ae59-baf499bb19df)
    
13. Verify users appear in **Active Directory Users and Computers** under **_EMPLOYEES**.
    ![image](https://github.com/user-attachments/assets/af751ccc-22f1-4920-b389-643f1600d5d4)
    ![image](https://github.com/user-attachments/assets/dfc2a9d4-faeb-4a30-8118-37717750b5f3)

*Note: In my case, I saw no change the first time I refreshed A.D. so I ran the script a second time, then I saw the desired result)*

---
### 🎉 Wrapping Up
Congratulations! You’ve successfully deployed and configured **Active Directory** in **Azure** with a fully functional **Domain Controller**, **Client Machine**, and **automated user creation**. This setup mirrors a real-world enterprise environment, demonstrating foundational IT administration skills.
