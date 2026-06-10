# Azure Virtual Machine Setup Guide — Free Tier

**Covers:** VM Creation • Configuration • Security • Connection • Bastion • Management

> **FREE TIER LIMITS:** Azure Free Tier includes 750 hours/month of B1s VM usage — Windows and Linux separately. Stay within these limits to avoid charges. Always deallocate VMs when not in use.

---

## What You Will Need

- An active Azure account with a Free Tier subscription
- A modern web browser (Microsoft Edge, Chrome, or Firefox)
- Internet connection
- An SSH client (for Linux VMs) — Windows 10/11 has built-in OpenSSH; alternatively use PuTTY
- Remote Desktop Connection (for Windows VMs) — built into all Windows editions

## Free Tier VM Specifications

| Setting | Free Tier Value |
|---|---|
| VM Size | B1s (1 vCPU, 1 GiB RAM) |
| Free Hours | 750 hours/month per OS type |
| OS Disk | Up to 64 GB (Standard SSD free) |
| Region | Choose the closest to you for best performance |
| Public IP | Dynamic (free); Static IP incurs charges |

---

## Part 1 — Creating a Windows Virtual Machine

Follow the steps below to deploy a Windows Server VM on the Azure Free Tier. The entire process takes approximately 10–15 minutes.

### Phase 1: Sign In to the Azure Portal

1. Open your web browser and navigate to `portal.azure.com` — the Microsoft Azure sign-in page will load
2. Enter your Microsoft account email address and click **Next**
3. Enter your password and complete any Multi-Factor Authentication (MFA) prompt
4. You are now on the **Azure Portal Home** dashboard

> **TIP:** Bookmark `portal.azure.com` for quick access. If you see a "Start with an Azure free trial" banner, your free credits are active.

### Phase 2: Create a New Virtual Machine

5. Click **Virtual machines** from the left sidebar, or type "Virtual machines" in the top search bar and select it
6. On the Virtual Machines page, click the **+ Create** button at the top left, then select **Azure virtual machine** — the Create a virtual machine wizard opens with multiple tabs

### Tab: Basics

**Step 7 — Subscription and Resource Group**

- Subscription: Your Azure Free Tier subscription should be pre-selected
- Resource group: Click **Create new**, enter a name (e.g. `MyWindowsRG`), and click OK

**Step 8 — Instance Details**

| Field | Recommended Value |
|---|---|
| Virtual machine name | e.g. `MyWindowsVM` (3–15 characters, letters/numbers/hyphens) |
| Region | Select the Azure region nearest to your physical location |
| Availability options | No infrastructure redundancy required |
| Security type | Standard |
| Image | Windows Server 2022 Datacenter — Gen2 |
| VM architecture | x64 |
| Size | B1s (listed as "Free services eligible") |

> **IMPORTANT:** When selecting VM Size, click "See all sizes", search for B1s, and confirm the "Free services eligible" label. Do NOT choose a larger size as it will incur charges.

**Step 9 — Administrator Account**

- Authentication type: Password
- Username: Enter an admin username (e.g. `azureuser`) — avoid using "admin" or "administrator"
- Password: Enter a strong password of at least 12 characters, including uppercase, lowercase, numbers, and symbols
- Confirm password: Re-enter the same password

> **SECURITY:** Save your username and password securely. You will need these to log into the VM using Remote Desktop. You cannot recover the password later — you can only reset it.

**Step 10 — Inbound Port Rules**

- Public inbound ports: Select **Allow selected ports**
- Select inbound ports: Check **RDP (3389)**
- This allows Remote Desktop Connection access to your Windows VM

> **NOTE:** Allowing RDP (port 3389) from the internet is acceptable for learning and testing. For production workloads, restrict access using IP allowlisting or Azure Bastion (covered in Part 5 of this guide).

11. Leave the Licensing section as-is and click **Next: Disks**

### Tab: Disks

12. OS disk type: Select **Standard SSD (locally-redundant storage)** — Standard SSD is included in the free tier; Premium SSD will incur charges
13. Encryption type: Leave as default (Platform-managed keys)
14. Click **Next: Networking**

### Tab: Networking

15. Virtual network: A new VNet is automatically created — leave the default name
16. Subnet: Leave as default
17. Public IP: A new dynamic IP is created automatically — leave as-is
18. NIC network security group: Select **Basic**; confirm RDP (3389) is still selected
19. Leave "Delete public IP and NIC when VM is deleted" unchecked
20. Click **Next: Management**

### Tab: Management

21. Enable auto-shutdown: Toggle this **ON**
22. Shutdown time: Set to a time when you are unlikely to be using the VM (e.g. 11:00 PM)
23. Time zone: Select your local time zone
24. Notification before shutdown: Enter your email to receive alerts

> **BEST PRACTICE:** Enabling auto-shutdown ensures the VM shuts down automatically if you forget to stop it, preventing your 750 free hours from being used up accidentally.

25. Leave all other settings as default and click **Next: Monitoring**

### Tab: Monitoring

26. Boot diagnostics: Leave as **Enable with managed storage account (recommended)**
27. Click **Review + create**

### Tab: Review + Create

28. Azure will validate your configuration — wait for the green **Validation passed** banner
29. Review the summary; confirm the VM size shows B1s and cost shows $0.00 or "Free"
30. Click the blue **Create** button to begin deployment (approximately 2–5 minutes)

> **TIP:** Do not close the browser tab during deployment. Azure sends a notification when deployment is complete.

31. When you see "Your deployment is complete", click **Go to resource**

---

## Part 2 — Connecting to Your Windows VM via Remote Desktop

Now that the VM is deployed, connect to it using Remote Desktop Protocol (RDP) for a full graphical desktop interface.

1. On the VM's Overview page, note the **Public IP address** displayed at the top right (e.g. `52.142.18.31`)
2. Click the **Connect** button at the top of the VM page, then select **RDP**
3. Confirm the IP address and port (3389) are correct
4. Click **Download RDP File** to download the `.rdp` configuration file to your computer
5. Open the downloaded `.rdp` file by double-clicking it; if prompted with a security warning, click **Connect**
6. In the Windows Security dialog, enter your admin username and password
7. Click **OK**; if a certificate warning appears, click **Yes** to accept and connect
8. The Windows Server desktop will load inside your Remote Desktop window

> **SUCCESS:** You are now connected to your Azure Windows VM. Server Manager will open automatically on first login.

**Managing the connection:**
- To disconnect: Click the X on the Remote Desktop window, or click Start > Disconnect
- Disconnecting does NOT stop the VM — it continues running and consuming free hours
- To stop the VM: Go to `portal.azure.com` > Virtual Machines > your VM > click **Stop**
- A stopped (deallocated) VM does NOT consume free hours

> **REMINDER:** Always STOP (deallocate) the VM when done. Simply disconnecting RDP keeps the VM running and will consume your 750 monthly free hours.

---

## Part 3 — Creating a Linux Virtual Machine

Linux VMs also qualify for 750 free hours per month on the B1s size — completely separate from the Windows free hours.

1. Open your browser, go to `portal.azure.com`, and sign in
2. Click **Virtual machines** in the left sidebar
3. Click **+ Create** and then select **Azure virtual machine**

### Tab: Basics

**Step 4 — Subscription and Resource Group**

- Subscription: Select your Free Tier subscription
- Resource group: Click **Create new**, name it (e.g. `MyLinuxRG`), and click OK

**Step 5 — Instance Details**

| Field | Recommended Value |
|---|---|
| Virtual machine name | e.g. `MyLinuxVM` (3–64 characters) |
| Region | Same region as your Windows VM, or nearest to you |
| Availability options | No infrastructure redundancy required |
| Security type | Standard |
| Image | Ubuntu Server 22.04 LTS — Gen2 |
| VM architecture | x64 |
| Size | B1s (free services eligible) |

**Step 6 — Administrator Account**

- Authentication type: Select **SSH public key** (recommended) or **Password**

> **RECOMMENDED:** SSH public key is more secure than a password for Linux VMs. Password is easier for beginners.

**Option A — SSH Public Key Authentication**

- Username: Enter a username (e.g. `azureuser`)
- SSH public key source: Select **Generate new key pair**
- Key pair name: Enter a name (e.g. `MyLinuxVM_key`)
- Azure will generate a key pair and offer to download the private key (`.pem` file) during VM creation

> **CRITICAL:** When prompted, click "Download private key and create resource". Save the `.pem` file to a secure location. You CANNOT download it again.

**Option B — Password Authentication**

- Username: Enter a username (e.g. `azureuser`)
- Password: Enter a strong password (12+ characters, mixed case, numbers, symbols)
- Confirm password: Re-enter the same password

7. Inbound Port Rules: Select **Allow selected ports** and check **SSH (22)**
8. Click **Next: Disks**

### Tab: Disks

9. OS disk type: Select **Standard SSD (locally-redundant storage)**
10. Click **Next: Networking** — leave all networking settings as default; a new VNet, subnet, and dynamic public IP will be created automatically

### Tab: Management

11. Enable auto-shutdown as described in Part 1 — set time, time zone, and notification email
12. Click **Review + create**

### Review + Create

13. Wait for the **Validation passed** banner
14. Confirm VM size shows B1s and cost estimate shows free or $0.00
15. Click **Create** — if you chose SSH key, the "Download private key and create resource" dialog appears; click it to download the `.pem` file and start deployment
16. Wait 2–5 minutes, then click **Go to resource**

---

## Part 4 — Connecting to Your Linux VM via SSH

### Finding the VM's Public IP Address

1. Go to `portal.azure.com` > Virtual Machines > select your Linux VM
2. On the Overview page, note the **Public IP address** (e.g. `20.50.120.45`)

### Method A: SSH Key (.pem file) — Windows 10/11

3. Open **Command Prompt** or **PowerShell** (press Windows + R, type `cmd`, press Enter)
4. Navigate to the folder where your `.pem` file was saved:
   ```
   cd C:\Users\YourName\Downloads
   ```
5. Set correct permissions on the `.pem` file (required):
   ```
   icacls MyLinuxVM_key.pem /inheritance:r /grant:r "%USERNAME%:(R)"
   ```
6. Connect using the SSH command:
   ```
   ssh -i MyLinuxVM_key.pem azureuser@YOUR_PUBLIC_IP
   ```
7. If prompted "Are you sure you want to continue connecting?", type `yes` and press Enter
8. The terminal prompt changes to: `azureuser@MyLinuxVM:~$` — you are connected

> **SUCCESS:** You are connected to your Azure Linux VM via SSH. You can now run Linux commands, install software, and manage the server.

### Method B: SSH Key — Using PuTTY

1. Download and install PuTTY from `putty.org`
2. Convert the `.pem` key to `.ppk` using PuTTYgen: Open PuTTYgen > click **Load** > select your `.pem` file > click **Save private key** > save as `.ppk`
3. Open PuTTY; in **Host Name**, enter: `azureuser@YOUR_PUBLIC_IP`
4. In the left panel: Connection > SSH > Auth > Credentials
5. Next to "Private key file for authentication", click **Browse** and select your `.ppk` file
6. Click **Open**, accept the security alert by clicking **Accept**

### Method C: Password Authentication

1. Open Command Prompt or PowerShell
2. Type: `ssh azureuser@YOUR_PUBLIC_IP`
3. When prompted, type `yes` and press Enter
4. Enter your password when prompted (the cursor will not move while typing — this is normal)
5. Press Enter — you are now connected

**Disconnecting:** Type `exit` at the terminal prompt and press Enter.

> **REMINDER:** Closing SSH does not stop the VM. Go to the Azure Portal and click **Stop** to deallocate it and preserve your free hours.

---

## Part 5 — Azure Bastion (Secure Browser-Based Access)

Azure Bastion provides secure, browser-based RDP and SSH access to your VMs directly through the Azure Portal — **without requiring a public IP, open RDP/SSH ports, or a VPN**. It is the recommended approach for production environments and eliminates the security risk of exposing port 3389 or 22 to the internet.

### How Azure Bastion Works

```
Your Browser (HTTPS/443)
        │
        ▼
  Azure Portal (portal.azure.com)
        │
        ▼
  Azure Bastion (deployed in AzureBastionSubnet)
        │  ← Private connection inside VNet
        ▼
  VM (no public IP required)
```

All traffic travels over TLS on port 443 — there is no need for the VM to have a public IP or open network ports for remote access.

### Bastion SKUs

| SKU | Features | Notes |
|---|---|---|
| **Basic** | RDP/SSH via browser; no native client support | Lower cost; sufficient for most lab/dev scenarios |
| **Standard** | Native client support; file transfer; shareable links; IP-based connections | Required for production or advanced use cases |

> **COST NOTE:** Azure Bastion is not included in the Free Tier. Basic SKU is billed per deployment hour plus data transfer. For lab use, deploy Bastion only when needed and delete it when done to minimise cost.

### Step 1: Create the AzureBastionSubnet

Azure Bastion requires a dedicated subnet named exactly `AzureBastionSubnet` (case-sensitive) with a minimum `/26` address range.

1. Go to `portal.azure.com` > search for **Virtual networks** > open your VNet (e.g. `vnet-firewall-lab` or the one created with your VM)
2. In the left sidebar, click **Subnets**
3. Click **+ Subnet** at the top
4. Configure the subnet:

| Field | Required Value |
|---|---|
| Name | `AzureBastionSubnet` (exact, case-sensitive) |
| Subnet address range | Minimum `/26` (e.g. `10.0.3.0/26`) — must not overlap existing subnets |
| NAT gateway | None |
| Network security group | None (Bastion manages its own NSG rules) |
| Route table | None |

5. Click **Save**

### Step 2: Deploy Azure Bastion

6. In the Azure Portal, search for **Bastions** and click it
7. Click **+ Create**
8. Configure the Bastion instance:

| Field | Recommended Value |
|---|---|
| Subscription | Your Free Tier subscription |
| Resource group | Same RG as your VM (e.g. `MyWindowsRG` or `MyLinuxRG`) |
| Name | e.g. `mybastion` |
| Region | Same region as your VM |
| Tier | Basic |
| Virtual network | Select your existing VNet |
| Subnet | `AzureBastionSubnet` (select from dropdown — it will auto-populate) |
| Public IP address | Create new; name it (e.g. `bastion-pip`); SKU: Standard |

9. Click **Review + create** and wait for the **Validation passed** banner
10. Click **Create** — Bastion deployment takes approximately 5–10 minutes

### Step 3: Connect to a Windows VM via Bastion

11. Go to `portal.azure.com` > **Virtual Machines** > open your Windows VM
12. On the VM Overview page, click **Connect** at the top, then select **Bastion**
13. On the Bastion connection panel, enter:
    - Username: Your admin username (e.g. `azureuser`)
    - Password: Your VM password
14. Click **Connect** — a new browser tab opens with a full RDP session to the VM, running entirely in your browser

> **NOTE:** If your Windows VM has no public IP assigned (best practice with Bastion), ensure the "Public IP address" field on the VM's Overview shows "None". Bastion connects through the private IP via the VNet.

### Step 4: Connect to a Linux VM via Bastion

15. Go to `portal.azure.com` > **Virtual Machines** > open your Linux VM
16. Click **Connect** > **Bastion**
17. On the Bastion connection panel, select the authentication type:
    - **Password**: Enter your username and password, then click **Connect**
    - **SSH Private Key from Local File**: Enter your username, click the file icon, and upload your `.pem` key, then click **Connect**
18. An SSH terminal opens directly in your browser — no local SSH client required

### Step 5: Harden VM Security After Deploying Bastion

Once Bastion is in place, you should remove the inbound RDP and SSH rules from the VM's Network Security Group (NSG) to close the public-facing ports:

19. Go to your VM > **Networking** in the left sidebar
20. Under **Inbound port rules**, find the rule allowing port 3389 (RDP) or port 22 (SSH)
21. Click the rule, then click **Delete** to remove it
22. Confirm deletion — the VM is now only reachable via Bastion, not directly from the internet

> **SECURITY BEST PRACTICE:** With Bastion deployed and RDP/SSH ports closed, your VM's attack surface is dramatically reduced. There are no open management ports exposed to the internet. All access is authenticated through the Azure Portal and travels over HTTPS.

### Bastion vs. Direct RDP/SSH — Comparison

| Feature | Direct RDP/SSH | Azure Bastion |
|---|---|---|
| Requires public IP on VM | Yes | No |
| Port 3389/22 exposed to internet | Yes | No |
| Access method | RDP client / SSH terminal | Browser (Azure Portal) |
| MFA support | Optional | Yes (via Azure AD) |
| Suitable for production | Risky without IP restrictions | Yes |
| Free Tier cost | Free (uses VM's public IP) | Billed per hour (not free) |
| Setup complexity | Low | Medium |

### Deleting Bastion When Not in Use

To avoid ongoing Bastion charges during lab work:

1. Go to `portal.azure.com` > **Bastions**
2. Select your Bastion instance
3. Click **Delete** and confirm
4. Also delete the associated Public IP: search **Public IP addresses** > find `bastion-pip` > Delete

> Deleting Bastion does not affect your VMs. You can redeploy it at any time following the steps above.

---

## Part 6 — Managing Your VMs in the Azure Portal

### Starting a Stopped VM

1. Go to `portal.azure.com` > **Virtual Machines** > click your VM
2. Click the **Start** button at the top and wait 1–2 minutes for the status to change to "Running"
3. Note the new Public IP address if it was set to Dynamic

### Stopping (Deallocating) a VM

1. Go to your VM > click **Stop** > confirm by clicking **Yes**
2. Status changes to "Stopped (deallocated)" — no compute charges apply

> **NOTE:** Only a **deallocated** VM avoids compute charges. Always use the Stop button in the Portal, not just the OS shutdown command.

### Restarting a VM

1. On the VM Overview page, click **Restart** and confirm — the VM reboots within 1–2 minutes

### Resetting the Password

1. On the VM page, scroll down the left sidebar and click **Reset password**
2. **Windows:** Enter a new username and/or password and click **Update**
3. **Linux:** Choose between "Reset SSH public key" or "Reset password" mode, fill in the details, and click **Update**

### Monitoring VM Usage

1. On the VM Overview page, scroll down to the **Monitoring** section
2. Live charts show CPU percentage, network in/out, and disk operations
3. Click any chart to expand it or adjust the time range

### Tracking Free Tier Usage

1. In the Azure Portal, search for **Cost Management + Billing**
2. Click **Cost Management** then **Cost analysis** — it should show $0.00 if you are within free limits
3. Alternatively, go to `portal.azure.com/home` and click **Free services** to see usage versus limits

---

## Part 7 — Troubleshooting Common Issues

### Cannot Connect via RDP (Windows VM)

- Ensure the VM status is **Running** in the Azure Portal
- Confirm RDP port 3389 is allowed in the Network Security Group (NSG) rules
- Check that your local firewall or antivirus is not blocking outbound RDP connections
- Try using the VM's IP address directly rather than the downloaded RDP file
- Verify you are using the correct username and password (passwords are case-sensitive)

### Cannot Connect via SSH (Linux VM)

- Ensure the VM status is **Running**
- Confirm port 22 (SSH) is allowed in the NSG
- If using a `.pem` key on Windows, ensure you set correct permissions with the `icacls` command
- If using PuTTY, verify the `.ppk` key is correctly converted and selected
- Try reconnecting — if a dynamic IP was assigned, the IP may have changed after a restart

### Cannot Connect via Bastion

- Confirm the `AzureBastionSubnet` name is exactly `AzureBastionSubnet` (case-sensitive)
- Ensure the subnet has at least a `/26` prefix (minimum 64 addresses)
- Verify that Bastion status is "Succeeded" in the portal
- Confirm the VM is in the same VNet as Bastion
- If connecting with SSH key, ensure the `.pem` file is the private key (not the public key)

### VM Deployment Fails

- **vCPU quota limits:** Switch to a different Azure region (e.g. from `westeurope` to `northeurope` or `eastus`)
- **Subscription validation errors:** Confirm your free trial subscription is active at `portal.azure.com/free`
- **Image not available in region:** Change the region and retry with the same image

### Free Hours Running Out Too Fast

- Stop (deallocate) VMs immediately when not in use
- Enable auto-shutdown on both VMs (Management tab during creation)
- Windows and Linux have separate 750-hour allowances — you can run both simultaneously within limits
- Check usage via Cost Management + Billing in the Portal

### RDP File Does Not Open

- Remote Desktop Connection is built into Windows 10/11 Home, Pro, and Enterprise
- **Mac:** Download the Microsoft Remote Desktop app from the Mac App Store
- **Linux:** Use Remmina or FreeRDP as the RDP client

---

## Quick Reference — Summary Tables

### Windows VM — Recommended Settings

| Setting | Value |
|---|---|
| Image | Windows Server 2022 Datacenter Gen2 |
| Size | B1s — Standard (Free) |
| Authentication | Password |
| Inbound Port | RDP (3389) — or none if using Bastion |
| OS Disk | Standard SSD |
| Auto-shutdown | Enabled |
| Connection Tool | Remote Desktop Connection (`mstsc`) or Azure Bastion |

### Linux VM — Recommended Settings

| Setting | Value |
|---|---|
| Image | Ubuntu Server 22.04 LTS Gen2 |
| Size | B1s — Standard (Free) |
| Authentication | SSH public key (recommended) or Password |
| Inbound Port | SSH (22) — or none if using Bastion |
| OS Disk | Standard SSD |
| Auto-shutdown | Enabled |
| Connection Tool | `ssh` command / PuTTY or Azure Bastion |

### Common Azure Portal Actions

| Action | Steps |
|---|---|
| Start VM | Portal > Virtual Machines > VM > Start |
| Stop VM | Portal > Virtual Machines > VM > Stop > Confirm |
| Restart VM | Portal > Virtual Machines > VM > Restart |
| Reset Password | Portal > VM > Reset password (left sidebar) |
| Connect via Bastion | Portal > VM > Connect > Bastion |
| Check Free Usage | Portal > Cost Management + Billing > Free services |
| Delete VM | Portal > VM > Delete (remove all resources) |
| Resize VM | Portal > VM > Size (left sidebar) |

---

> **FINAL REMINDER:** To preserve your Azure Free Tier credits: (1) Always stop VMs when not in use, (2) Use B1s size only, (3) Enable auto-shutdown, (4) Use Azure Bastion instead of open RDP/SSH ports where possible, (5) Monitor usage in Cost Management.
