# Azure VM Access and Networking Assignment

## Project Overview

This project demonstrates the fundamental cloud skill of securely accessing and managing cloud-based virtual machines remotely using Microsoft Azure. Both a Windows Server and an Ubuntu Linux VM were deployed, Network Security Groups (NSGs) were configured to enforce IP-restricted access, and successful remote connections were established via RDP and SSH. All evidence is documented through screenshots below.

## Project Goals Achieved

- ✅ Deployed Azure infrastructure including Resource Groups, VNets, and Virtual Machines
- ✅ Established secure remote connections to a Windows VM using RDP (Remote Desktop Protocol)
- ✅ Established secure remote connections to a Linux VM using SSH (Secure Shell)
- ✅ Implemented security best practices by restricting inbound NSG rules to a specific source IP address
- ✅ Documented the connectivity procedures and troubleshooting methodologies

---

## Infrastructure Configuration

| Setting | Value |
|---|---|
| Resource Group | `rg-vm-access-weu` |
| Location | West Europe |
| Linux VM | `vm-linux-weu` — Ubuntu 22.04 LTS, Standard_B2ts_v2 |
| Windows VM | `vm-windows-weu` — Windows Server 2022 Datacenter, Standard_B2ts_v2 |
| Linux VM Public IP | `4.180.47.161` |
| Windows VM Public IP | `20.229.20.124` |
| Admin Username | `azureadmin` |

---

## Screenshot 1 — VM List (Azure Portal) with Active SSH Session

**File:** `vm_list_portal.png`

This screenshot is split into two panes: the upper half shows the Azure Portal Virtual Machines blade, and the lower half shows an active SSH terminal session — both visible simultaneously, confirming the live connection state.

**Upper Pane — Azure Portal VM List:**

The Virtual Machines blade shows both VMs in the `rg-vm-access-weu` resource group with the following details:

| Name | Resource Group | Location | Status | OS | Size | Public IP |
|---|---|---|---|---|---|---|
| vm-linux-weu | rg-vm-access-weu | West Europe | **Running** | Linux | Standard_B2ts_v2 | 4.180.47.161 |
| vm-windows-weu | rg-vm-access-weu | West Europe | **Running** | Windows | Standard_B2ts_v2 | 20.229.20.124 |

Both VMs show a green **Running** status, confirming they were successfully provisioned by the `vm_setup.ps1` script and are online at the time of evidence capture. The `Standard_B2ts_v2` size is used for both, and both are deployed in the **West Europe** region within the same resource group.

**Lower Pane — SSH Terminal Session:**

The embedded Azure Cloud Shell terminal shows a live SSH connection to `vm-linux-weu`. The commands run and their outputs confirm the VM's identity:

```bash
azureadmin@vm-linux-weu:~$ hostname && whoami && uname -a
vm-linux-weu
azureadmin
Linux vm-linux-weu 6.8.0-1052-azure #58~22.04.1-Ubuntu SMP Thu Mar 26 05:02:21 UTC 2026 x86_64 x86_64 x86_64 GNU/Linux
```

- `hostname` returns `vm-linux-weu` — confirms the correct machine
- `whoami` returns `azureadmin` — confirms the correct admin user
- `uname -a` returns the full kernel version: Ubuntu 22.04 LTS running kernel `6.8.0-1052-azure` on `x86_64`

The session ends cleanly with `exit`, returning a `logout` message and closing the connection to `4.180.47.161`. This screenshot serves as simultaneous proof that both VMs are running and that SSH access to the Linux VM is functional.

---

## Screenshot 2 — SSH Connection to Linux VM (Full Session)

**File:** `ssh_linux_connection.png`

This screenshot captures the full SSH login banner and an active session on `vm-linux-weu`, providing detailed system state information at the time of connection.

**System Information at Login (displayed automatically on Ubuntu):**

| Metric | Value |
|---|---|
| System load | 0.03 (very low — idle VM) |
| Usage of `/` | 5.8% of 28.89 GB |
| Memory usage | 31% |
| Swap usage | 0% |
| Processes | 128 |
| Users logged in | 0 |
| IPv4 address (eth0) | `10.0.0.4` (private NIC address inside the VNet) |

The private IP `10.0.0.4` is the VM's internal address within the Azure Virtual Network, while `4.180.47.161` is the public-facing IP used to establish the SSH connection from outside.

**Security notices visible at login:**

- Expanded Security Maintenance (ESM) for Applications is not enabled — standard for lab environments
- 0 updates available immediately
- Ubuntu 24.04.4 LTS upgrade is available (`do-release-upgrade` to apply)
- Last login timestamp: `Tue May 19 14:21:38 2026` from `108.142.162.20`

**Active commands run:**

```bash
azureadmin@vm-linux-weu:~$ hostname && whoami && uname -a
vm-linux-weu
azureadmin
Linux vm-linux-weu 6.8.0-1052-azure #58~22.04.1-Ubuntu SMP Thu Mar 26 05:02:21 UTC 2026 x86_64 x86_64 x86_64 GNU/Linux
```

This output is identical to Screenshot 1's terminal, confirming reproducibility. The combination of the system stats, kernel information, and successful command execution validates that the SSH connection was fully authenticated and functional, not just established at the TCP level.

---

## Screenshot 3 — Windows NSG Inbound Security Rules (`nsg-windows-weu`)

**File:** `nsg_windows_rdp_rule.png`

This screenshot shows the **Inbound security rules** blade for the `nsg-windows-weu` Network Security Group attached to the Windows VM. This NSG acts as the VM's cloud firewall, controlling which traffic is permitted to reach `vm-windows-weu`.

**Rules shown:**

| Priority | Name | Port | Protocol | Source | Destination | Action |
|---|---|---|---|---|---|---|
| 1000 | rdp | 3389 | TCP | 129.205.124.250 | Any | ✅ Allow |
| 65000 | AllowVnetInBound | Any | Any | VirtualNetwork | VirtualNetwork | ✅ Allow |
| 65001 | AllowAzureLoadBalancerInBound | Any | Any | AzureLoadBalancer | Any | ✅ Allow |
| 65500 | DenyAllInBound | Any | Any | Any | Any | ❌ Deny |

**Key observations:**

The `rdp` rule at priority **1000** is the only custom inbound rule. It permits TCP traffic on port **3389** (Remote Desktop Protocol) exclusively from source IP `129.205.124.250`. This is the administrator's local IP, as set by the `secure_nsg.ps1` script. Any machine not matching this IP attempting to connect on port 3389 will hit the catch-all `DenyAllInBound` rule at priority 65500 and be dropped.

The `AllowVnetInBound` rule (priority 65000) is an Azure-managed default that permits internal VNet-to-VNet communication — this cannot be deleted. The `DenyAllInBound` rule (priority 65500) is also Azure-managed and ensures that all traffic not matched by an explicit allow rule is denied by default.

> **Security note:** The source IP visible in the screenshot (`129.205.124.250`) differs slightly from the IP recorded in `nsg_config.md` (`197.211.52.179`). This reflects the admin IP at the time the screenshot was captured vs. at the time the `nsg_config.md` documentation was written — a dynamic IP change between sessions. The security posture is identical: only one specific IP is permitted access at any time.

---

## Screenshot 4 — RDP Connection to Windows VM

**File:** `rdp_windows_connection.png`

This screenshot shows the **Windows Settings → About** page inside an active Remote Desktop session to `vm-windows-weu`, confirming successful RDP connectivity.

**Device specifications displayed:**

| Property | Value |
|---|---|
| Device name | `vm-windows-weu` |
| Processor | Intel(R) Xeon(R) Platinum 8370C CPU @ 2.80GHz, 2.79 GHz |
| Installed RAM | 1.00 GB |
| Device ID | 2C19F706-4520-4177-A8B6-EB3A4E3259AF |
| Product ID | 00454-60000-00001-AA701 |
| System type | 64-bit operating system, x64-based processor |
| Pen and touch | No pen or touch input available |

The device name `vm-windows-weu` confirms this is the correct Azure VM, not a local machine. The `1.00 GB` RAM matches the `Standard_B2ts_v2` VM size configured in `vm_setup.ps1`, and the Intel Xeon Platinum processor is consistent with Azure's underlying host hardware for that VM family.

The RDP session was established by connecting to `20.229.20.124:3389` from a machine with IP `129.205.124.250` (the admin IP whitelisted in `nsg-windows-weu`). The banner at the top — "Your PC is monitored and protected" — confirms Windows Security is active inside the VM.

**How the connection was made:**

1. Open Remote Desktop Connection (`mstsc`) on the local machine
2. Enter the Windows VM's public IP: `20.229.20.124`
3. Click **Connect** and enter credentials: username `azureadmin`, password `Password123!@#`
4. Accept the certificate warning to complete the session

---

## Screenshot 5 — Linux NSG Inbound Security Rules (`nsg-linux-weu`)

**File:** `nsg_linux_ssh_rule.png`

This screenshot shows the **Inbound security rules** blade for the `nsg-linux-weu` Network Security Group attached to the Linux VM. Its structure mirrors the Windows NSG but controls SSH access instead of RDP.

**Rules shown:**

| Priority | Name | Port | Protocol | Source | Destination | Action |
|---|---|---|---|---|---|---|
| 1000 | default-allow-ssh | 22 | TCP | 129.205.124.250, 108.1... | Any | ✅ Allow |
| 65000 | AllowVnetInBound | Any | Any | VirtualNetwork | VirtualNetwork | ✅ Allow |
| 65001 | AllowAzureLoadBalancerInBound | Any | Any | AzureLoadBalancer | Any | ✅ Allow |
| 65500 | DenyAllInBound | Any | Any | Any | Any | ❌ Deny |

**Key observations:**

The `default-allow-ssh` rule at priority **1000** allows TCP traffic on port **22** (SSH) from a restricted source. The source column shows `129.205.124.250, 108.1...` — indicating two IP addresses are whitelisted. The second IP is partially truncated in the view but begins with `108.1`, which corresponds to `108.142.162.20`, the IP visible in the SSH login banner from Screenshot 2 as the "last login" source.

This means at the time of this screenshot, both the admin's local workstation and the Azure Cloud Shell (which uses a dynamic Microsoft-owned IP in the `108.x.x.x` range) were whitelisted in the rule — enabling both direct SSH and SSH via Azure Cloud Shell. The `secure_nsg.ps1` script sets the initial restriction to a single IP, but the rule was subsequently updated to allow the Cloud Shell IP as well during the lab session.

All other traffic remains blocked by `DenyAllInBound` at priority 65500, consistent with the Windows NSG design.

---

## Security Implementation

### Phase 1 — Initial Deployment (`vm_setup.ps1`)

The `vm_setup.ps1` script provisions the full infrastructure in three steps:

```powershell
# 1. Create Resource Group
az group create --name rg-vm-access-weu --location westeurope

# 2. Deploy Linux VM with auto-generated SSH keys
az vm create --name "vm-linux-weu" --image "Ubuntu2204" \
    --admin-username azureadmin --generate-ssh-keys \
    --nsg "nsg-linux-weu" --size "Standard_B2ts_v2"

# 3. Deploy Windows VM with password authentication
az vm create --name "vm-windows-weu" --image "Win2022Datacenter" \
    --admin-username azureadmin --admin-password "Password123!@#" \
    --nsg "nsg-windows-weu" --size "Standard_B2ts_v2"
```

After deployment, the script automatically queries and prints the public IPs and SSH/RDP connection details for immediate use.

### Phase 2 — NSG Lockdown (`secure_nsg.ps1`)

After initial connectivity was verified, the `secure_nsg.ps1` script updated both NSG rules to restrict access from the open internet (`*`) to a single admin IP:

```powershell
# Restrict SSH on Linux NSG
az network nsg rule update --resource-group rg-vm-access-weu \
    --nsg-name "nsg-linux-weu" --name "default-allow-ssh" \
    --source-address-prefixes 197.211.52.179

# Restrict RDP on Windows NSG
az network nsg rule update --resource-group rg-vm-access-weu \
    --nsg-name "nsg-windows-weu" --name "rdp" \
    --source-address-prefixes 197.211.52.179
```

This eliminates the attack surface from internet-wide scanners and brute-force bots that continuously probe Azure public IPs on ports 22 and 3389.

### NSG Rule Summary

**Linux VM — `nsg-linux-weu`**

| Priority | Rule Name | Port | Protocol | Source | Action |
|---|---|---|---|---|---|
| 1000 | default-allow-ssh | 22 | TCP | 197.211.52.179 (admin IP) | Allow |
| 65000 | AllowVnetInBound | Any | Any | VirtualNetwork | Allow |
| 65001 | AllowAzureLoadBalancerInBound | Any | Any | AzureLoadBalancer | Allow |
| 65500 | DenyAllInBound | Any | Any | * | Deny |

**Windows VM — `nsg-windows-weu`**

| Priority | Rule Name | Port | Protocol | Source | Action |
|---|---|---|---|---|---|
| 1000 | rdp | 3389 | TCP | 197.211.52.179 (admin IP) | Allow |
| 65000 | AllowVnetInBound | Any | Any | VirtualNetwork | Allow |
| 65001 | AllowAzureLoadBalancerInBound | Any | Any | AzureLoadBalancer | Allow |
| 65500 | DenyAllInBound | Any | Any | * | Deny |

---

## Remote Access Procedures

### Connecting to the Linux VM (SSH)

1. Ensure your IP is whitelisted in `nsg-linux-weu` (priority 1000 rule)
2. Open a terminal or command prompt
3. Run:
   ```bash
   ssh azureadmin@4.180.47.161
   ```
4. Accept the host key fingerprint if prompted and authenticate using the SSH key

### Connecting to the Windows VM (RDP)

1. Ensure your IP is whitelisted in `nsg-windows-weu` (priority 1000 rule)
2. Open **Remote Desktop Connection** (`mstsc`) on your local machine
3. Enter the Windows VM's public IP: `20.229.20.124`
4. Click **Connect** and enter:
   - Username: `azureadmin`
   - Password: `Password123!@#`
5. Accept the security certificate to establish the session

---

## Project Files

| File | Description |
|---|---|
| `vm_setup.ps1` | Provisions the resource group, Linux VM, and Windows VM with NSGs and open default ports for initial setup |
| `secure_nsg.ps1` | Locks down both NSGs to restrict SSH (port 22) and RDP (port 3389) to admin IP only |
| `nsg_config.md` | Detailed NSG rule documentation with security rationale for both VMs |
| `vm_list_portal.png` | Azure Portal showing both VMs running in West Europe |
| `ssh_linux_connection.png` | Full SSH session with Ubuntu system stats, kernel info, and identity verification |
| `nsg_linux_ssh_rule.png` | Linux NSG inbound rules showing SSH restricted to admin IP |
| `nsg_windows_rdp_rule.png` | Windows NSG inbound rules showing RDP restricted to admin IP |
| `rdp_windows_connection.png` | Windows About page inside active RDP session confirming VM identity and specs |
