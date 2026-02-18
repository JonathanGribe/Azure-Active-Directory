



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

## **2.3 Restrict local administrator membership**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Restricted Groups**  
2. Add the **Administrators** group.  
3. Specify only approved administrative groups or accounts.

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

---

# **4. Session management (3.3)**

## **4.1 Configure workstation lock timeout**

1. Navigate to:  
   **User Configuration** > **Policies** > **Administrative Templates** > **Control Panel** > **Personalization**  
2. Configure:  
   - **Enable screen saver**: Enabled  
   - **Password protect the screen saver**: Enabled  
   - **Screen saver timeout**: 600 seconds  

## **4.2 Configure server lock timeout**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Administrative Templates** > **System** > **Group Policy**  
2. Set **User Group Policy loopback processing mode** to **Enabled**.  
3. Navigate to:  
   **User Configuration** > **Policies** > **Administrative Templates** > **Control Panel** > **Personalization**  
4. Set **Screen saver timeout** to **900 seconds**.

---

# **5. System configuration and hardening (3.4)**

## **5.1 Enable Windows Firewall**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Windows Defender Firewall with Advanced Security**  
2. For each profile (Domain, Private, Public), set **Firewall state** to **On**.

## **5.2 Disable USB storage devices**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Administrative Templates** > **System** > **Removable Storage Access**  
2. Enable:  
   - **Removable Disks: Deny read access**  
   - **Removable Disks: Deny write access**

## **5.3 Restrict unauthorized software installation**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Application Control Policies**  
2. Configure **AppLocker** or **Software Restriction Policies** to allow only approved applications.

---

# **6. Logging and monitoring (3.5)**

## **6.1 Configure event log retention**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Event Log**  
2. Configure the **Security** log:  
   - **Maximum log size**: Set according to retention needs  
   - **Retention method**: **Overwrite events as needed**

## **6.2 Enable advanced auditing**

1. Navigate to:  
   **Computer Configuration** > **Policies** > **Windows Settings** > **Security Settings** > **Advanced Audit Policy Configuration** > **Audit Policies**  
2. Enable auditing for:  
   - Account Logon  
   - Logon/Logoff  
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





