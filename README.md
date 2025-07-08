# winserver2022-domain-controller-udm-pro
# Post-Installation Setup for Windows Server 2022 as Domain Controller (with UDM Pro Integration)

This guide covers the essential post-installation steps to configure **Windows Server 2022** as a **Domain Controller (DC)**, including integration with a **UniFi Dream Machine Pro (UDM Pro)** for DHCP and network management.

---

## Prerequisites

- Windows Server 2022 installed and powered on
- Static IP assigned to the server
- vSphere environment (optional, if VM-based)
- Access to UniFi Dream Machine Pro admin portal

---

## Step 1: Install Required Roles and Features

1. Open **Server Manager**.
2. Click **Manage > Add Roles and Features**.
3. Select the following roles:
   - **Active Directory Domain Services (AD DS)**
   - **DNS Server**
   - **DHCP Server**
4. Complete the installation and **restart the server** if prompted.

---

## Step 2: Promote Server to Domain Controller

1. In **Server Manager**, click the **flag notification** and select **Promote this server to a domain controller**.
2. Choose **Add a new forest**.
3. Specify the **Root domain name** (e.g., `corp.local`).
4. Set the **Directory Services Restore Mode (DSRM) password**.
5. Accept default options for DNS delegation and NetBIOS name.
6. Complete the wizard and allow the server to reboot.

---

## Step 3: Configure DHCP Scope

1. Open **Server Manager > Tools > DHCP**.
2. Expand your server, right-click **IPv4**, and select **New Scope**.
3. Use the wizard to define:
   - **Name** (e.g., CorpScope)
   - **IP address range** (e.g., 192.168.1.100 to 192.168.1.200)
   - **Subnet mask** (e.g., 255.255.255.0)
   - **Default gateway** (e.g., 192.168.1.1)
   - **Lease duration** (default or customized)
   - **DNS server** (set to your DC's IP address)
4. Activate the scope once configured.

---

## Step 4: Configure DNS Reverse Lookup Zone

1. Open **Server Manager > Tools > DNS**.
2. Right-click **Reverse Lookup Zones** → **New Zone**.
3. Use the wizard:
   - Select **Primary Zone**.
   - Enter **Network ID** (e.g., `192.168.1`).
   - Complete the wizard.
4. Add a **PTR record** for your Domain Controller.

---

## Step 5: UniFi Dream Machine Pro (UDM Pro) Configuration

1. Log in to your **UDM Pro Controller**.
2. Go to **Settings > Networks**.
3. Select the network/VLAN used by your server.
4. Set **DHCP Mode** to **DHCP Server** if you want UDM Pro to manage DHCP alongside or outside of AD DHCP.
5. Define DHCP IP range, making sure it **does not overlap** with your AD DHCP scope to avoid conflicts.
6. Save your settings.

> **Note:**  
> Running DHCP both on the Domain Controller and UDM Pro requires careful subnet or VLAN separation.

---

## Step 6: Configure Domain Controller IP Settings

1. On the Domain Controller, open **Network Settings**.
2. Assign a **static IP address** if not already set.
3. Set the **Preferred DNS server** to the Domain Controller's own IP address.

---

## Step 7: Enable Remote Desktop (RDP)

1. Open **Server Manager**.
2. Click on **Local Server**.
3. Set **Remote Desktop** to **Enabled**.
4. Configure Windows Firewall to allow RDP connections if prompted.

---

## Step 8: Join Client Machines to the Domain

1. On each client:
   - Set DNS server to the Domain Controller’s IP address.
   - Open **System Properties** > **Computer Name** > **Change**.
   - Select **Domain**, enter your domain name (e.g., `corp.local`).
   - Provide domain admin credentials.
2. Restart the client machine to apply changes.

---

## Summary

| Task                          | Status      |
|-------------------------------|-------------|
| AD DS, DNS, DHCP Installed    | ✅ Completed |
| Domain Controller Promoted    | ✅ Completed |
| DHCP Scope Configured         | ✅ Completed |
| DNS Reverse Lookup Zone Set   | ✅ Completed |
| UDM Pro DHCP Configured       | ✅ Completed |
| Static IP & DNS Set on DC     | ✅ Completed |
| Remote Desktop Enabled        | ✅ Completed |
| Client Machines Joined Domain | ✅ Completed |

---

## References

- [Microsoft Docs: Active Directory Domain Services](https://learn.microsoft.com/en-us/windows-server/identity/active-directory-domain-services)
- [UniFi UDM Pro Documentation](https://help.ui.com/hc/en-us/articles/360052310753-UniFi-Dream-Machine-Pro)


