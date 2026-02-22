



# **Group Policy Configuration Steps**

The following procedures describe how to configure Group Policy Objects (GPOs) to implement the security controls defined in sections 3.1 through 3.7.

---

## **1. Create and link a Group Policy Object**

1. Open **Group Policy Management**.  
2. In the console tree, expand **Forest:** *domain name*, expand **Domains**, and select your domain.  
3. Right‑click **Group Policy Objects**, and select **New**.  
4. Enter a name for the GPO, and select **OK**.  
5. Right‑click the GPO, and select **Edit**.  
6. Link the GPO to the appropriate OU by right‑clicking the OU and selecting **Link an Existing GPO**.

---

# **2. Access control (3.1)**

## **2.1 Configure audit policies**

1. Open the GPO.  
2. Navigate to:  
   **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Local Policies** > **Audit Policy**  
3. Enable:  
   - **Audit account logon events**: Success, Failure  
   - **Audit logon events**: Success, Failure
  
  <img width="460" height="210" alt="image" src="https://github.com/user-attachments/assets/c3b09bad-4949-4291-90e8-bdbf4fd86ade" />


## **2.2 Require Network Level Authentication (NLA)**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Remote Desktop Services** > **Remote Desktop Session Host** > **Security**  
2. Set **Require user authentication for remote connections by using Network Level Authentication** to **Enabled**.

<img width="601" height="240" alt="image" src="https://github.com/user-attachments/assets/6532c061-f2b1-430e-ae15-019d0feaaf26" />


## **2.3 Restrict local administrator membership**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Restricted Groups**  
2. Add the **Administrators** group.  
3. Specify only approved administrative groups or accounts.

<img width="377" height="525" alt="image" src="https://github.com/user-attachments/assets/d25599cc-51e8-46d6-8ab2-a80ddd90d150" />

---

# **3. Password requirements (3.2)**

> **Note:** Configure these settings in a GPO linked at the domain root (typically the **Default Domain Policy**).

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Account Policies** > **Password Policy**  
2. Configure:  
   - **Minimum password length**: 14  
   - **Password must meet complexity requirements**: Enabled  
   - **Maximum password age**: 180 days  
   - **Enforce password history**: 24 passwords  

<img width="523" height="207" alt="image" src="https://github.com/user-attachments/assets/958480ef-7df0-427b-b807-02c5d728bf88" />

---

# **4. Session management (3.3)**

## **4.1 Configure workstation lock timeout**

1. Navigate to:  
   **User Configuration** > **Policies** > **Administrative Templates** > **Control Panel** > **Personalization**  
2. Configure:  
   - **Enable screen saver**: Enabled  
   - **Password protect the screen saver**: Enabled  
   - **Screen saver timeout**: 600 seconds
  
   <img width="577" height="358" alt="image" src="https://github.com/user-attachments/assets/cd76511b-6f4b-4cf1-a925-92e76afe9121" />


## **4.2 Configure server lock timeout**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Administrative Templates** > **System** > **Group Policy**  
2. Set **User Group Policy loopback processing mode** to **Enabled**.  
3. Navigate to:  
   **User Configuration** > **Policies** > **Administrative Templates** > **Control Panel** > **Personalization**  
4. Set **Screen saver timeout** to **900 seconds**.
<img width="606" height="385" alt="image" src="https://github.com/user-attachments/assets/0b77c4fe-00ca-4cec-94fd-f472df9a1de0" />

---

# **5. System configuration and hardening (3.4)**

## **5.1 Enable Windows Firewall**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Windows Defender Firewall with Advanced Security**  
2. For each profile (Domain, Private, Public), set **Firewall state** to **On**.

<img width="810" height="555" alt="image" src="https://github.com/user-attachments/assets/e6da2388-f61a-4b33-ad62-228b9e578a44" />


## **5.2 Disable USB storage devices**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Administrative Templates** > **System** > **Removable Storage Access**  
2. Enable:  
   - **Removable Disks: Deny read access**  
   - **Removable Disks: Deny write access**
  
<img width="580" height="394" alt="image" src="https://github.com/user-attachments/assets/33ed8f51-adf6-4892-b19f-851e79bf26e6" />

## **5.3 Restrict unauthorized software installation**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Application Control Policies**  
2. Configure **AppLocker** or **Software Restriction Policies** to allow only approved applications.

<img width="489" height="740" alt="image" src="https://github.com/user-attachments/assets/5d5d844b-ad4c-40cc-8b32-abcbb5cd93a3" />

More on AppLocker: https://learn.microsoft.com/en-us/windows/security/application-security/application-control/app-control-for-business/applocker/applocker-overview

---

# **6. Logging and monitoring (3.5)**

## **6.1 Configure event log retention**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Event Log**  
2. Configure the **Security** log:  
   - **Maximum log size**: Set according to retention needs  
   - **Retention method**: **Overwrite events as needed**

  <img width="453" height="269" alt="image" src="https://github.com/user-attachments/assets/e1efbe65-9ad8-4a2f-8dab-6a7b42042c63" />


## **6.2 Enable advanced auditing**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Advanced Audit Policy Configuration** > **Audit Policies**  
2. Enable auditing for:  
   - Account Logon
<img width="449" height="372" alt="image" src="https://github.com/user-attachments/assets/4b8071ae-5247-4579-8a58-f3a99960525e" />


   - Logon/Logoff
<img width="431" height="266" alt="image" src="https://github.com/user-attachments/assets/a83dcb3f-3675-450e-99e0-55d842455b75" />

   - Account Management  
   - Privilege Use  
   - Policy Change  
   - System

  

---

# **7. Patch and vulnerability management (3.6)**

## **7.1 Configure Windows Update settings**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Windows Update**  
2. Configure:  
   - **Configure Automatic Updates**: Enabled  
   - **Specify intranet Microsoft update service location**: Set WSUS server URL  

---

# **8. Acceptable use (3.7)**

## **8.1 Configure logon banner**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Local Policies** > **Security Options**  
2. Configure:  
   - **Interactive logon: Message title for users attempting to log on**  
   - **Interactive logon: Message text for users attempting to log on**





