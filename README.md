# Azure VM Access and Networking Assignment

## Project Overview
This project focuses on the fundamental cloud skill of securely accessing and managing cloud-based virtual machines remotely using Microsoft Azure. I deployed both Windows and Linux virtual machines, configured Network Security Groups (NSGs) for secure remote management, and demonstrated cross-platform connectivity using industry-standard protocols (RDP and SSH).

## Project Goals Achieved
* ✅ Deployed Azure infrastructure including Resource Groups, VNets, and Virtual Machines.
* ✅ Established secure remote connections to a Windows VM using RDP (Remote Desktop Protocol).
* ✅ Established secure remote connections to a Linux VM using SSH (Secure Shell).
* ✅ Implemented security best practices by restricting inbound NSG rules to a specific source IP address.
* ✅ Documented the connectivity procedures and troubleshooting methodologies.

---

## Infrastructure Configuration

- **Resource Group:** `rg-vm-access-weu`
- **Location:** West Europe
- **Linux VM:** `vm-linux-weu` (Ubuntu 22.04 LTS, Standard_B2ts_v2)
- **Windows VM:** `vm-windows-weu` (Windows Server 2022 Datacenter, Standard_B2ts_v2)

---

## Remote Access Procedures

### 1. Connecting to the Linux VM (SSH)
1. Ensure your IP address is whitelisted in the NSG (`nsg-linux-weu`).
2. Open a terminal or command prompt.
3. Use the SSH command with the VM's public IP:
   ```bash
   ssh azureadmin@4.180.47.161
   ```
4. Accept the host key fingerprint if prompted, and authenticate using the generated SSH key.

### 2. Connecting to the Windows VM (RDP)
1. Ensure your IP address is whitelisted in the NSG (`nsg-windows-weu`).
2. Open the **Remote Desktop Connection** client on your local machine.
3. Enter the Windows VM's Public IP: `20.229.20.124`.
4. Click **Connect** and enter the credentials:
   - **Username:** `azureadmin`
   - **Password:** `Password123!@#`
5. Accept the security certificate warning to establish the remote desktop session.

---

## Security Implementation: NSG Configuration

Initially, the Virtual Machines were provisioned with default open ports for RDP (3389) and SSH (22) to the entire internet (`*`). To adhere to cloud security best practices, the Network Security Group rules were updated to restrict inbound management traffic strictly to my local IP address.

### Security Script Execution (`secure_nsg.ps1`)
The provided PowerShell script automates the lockdown of both NSGs:
- Updates `default-allow-ssh` rule on `nsg-linux-weu` to only allow traffic from `197.211.52.179`.
- Updates `default-allow-rdp` rule on `nsg-windows-weu` to only allow traffic from `197.211.52.179`.

This prevents unauthorized scanning and brute-force attacks from malicious actors on the internet.

---

## Evidence

### SSH Connection to Linux VM
![SSH Connection to vm-linux-weu](screenshots/ssh_linux_connection.png)
*Successful SSH session — hostname `vm-linux-weu`, user `azureadmin`, Ubuntu 22.04 LTS*

### VM Inventory (Azure Portal)
![Azure Portal VM List](screenshots/vm_list_portal.png)
*Both VMs running in West Europe on Standard_B2ts_v2*

### Linux NSG — SSH Rule Restricted to Admin IP
![Linux NSG SSH Rule](screenshots/nsg_linux_ssh_rule.png)
*Port 22 restricted to 197.211.52.179 only*

### Windows NSG — RDP Rule Restricted to Admin IP
![Windows NSG RDP Rule](screenshots/nsg_windows_rdp_rule.png)
*Port 3389 restricted to 197.211.52.179 only*

### RDP Connection to Windows VM
![RDP Connection to vm-windows-weu](screenshots/rdp_windows_connection.png)
*Successful RDP session to `vm-windows-weu` at `20.229.20.124` using credentials `azureadmin`*

---

## Project Files

| File | Description |
|------|-------------|
| `vm_setup.ps1` | Provisions the resource group, Linux VM, and Windows VM with open NSG rules for initial setup |
| `secure_nsg.ps1` | Locks down both NSGs to restrict SSH and RDP to admin IP only |
| `nsg_config.md` | Detailed NSG rule documentation and security rationale |
| `screenshots/` | Evidence screenshots of VMs, connections, and NSG configurations |


# Azure VM Access and Networking Assignment

## Project Overview
This project focuses on the fundamental cloud skill of securely accessing and managing cloud-based virtual machines remotely using Microsoft Azure. I deployed both Windows and Linux virtual machines, configured Network Security Groups (NSGs) for secure remote management, and demonstrated cross-platform connectivity using industry-standard protocols (RDP and SSH).

## Project Goals Achieved
* ✅ Deployed Azure infrastructure including Resource Groups, VNets, and Virtual Machines.
* ✅ Established secure remote connections to a Windows VM using RDP (Remote Desktop Protocol).
* ✅ Established secure remote connections to a Linux VM using SSH (Secure Shell).
* ✅ Implemented security best practices by restricting inbound NSG rules to a specific source IP address.
* ✅ Documented the connectivity procedures and troubleshooting methodologies.

---

## Infrastructure Configuration

- **Resource Group:** `rg-vm-access-weu`
- **Location:** West Europe
- **Linux VM:** `vm-linux-weu` (Ubuntu 22.04 LTS, Standard_B2ts_v2)
- **Windows VM:** `vm-windows-weu` (Windows Server 2022 Datacenter, Standard_B2ts_v2)

---

## Remote Access Procedures

### 1. Connecting to the Linux VM (SSH)
1. Ensure your IP address is whitelisted in the NSG (`nsg-linux-weu`).
2. Open a terminal or command prompt.
3. Use the SSH command with the VM's public IP:
   ```bash
   ssh azureadmin@4.180.47.161
   ```
4. Accept the host key fingerprint if prompted, and authenticate using the generated SSH key.

### 2. Connecting to the Windows VM (RDP)
1. Ensure your IP address is whitelisted in the NSG (`nsg-windows-weu`).
2. Open the **Remote Desktop Connection** client on your local machine.
3. Enter the Windows VM's Public IP: `20.229.20.124`.
4. Click **Connect** and enter the credentials:
   - **Username:** `azureadmin`
   - **Password:** `Password123!@#`
5. Accept the security certificate warning to establish the remote desktop session.

---

## Security Implementation: NSG Configuration

Initially, the Virtual Machines were provisioned with default open ports for RDP (3389) and SSH (22) to the entire internet (`*`). To adhere to cloud security best practices, the Network Security Group rules were updated to restrict inbound management traffic strictly to my local IP address.

### Security Script Execution (`secure_nsg.ps1`)
The provided PowerShell script automates the lockdown of both NSGs:
- Updates `default-allow-ssh` rule on `nsg-linux-weu` to only allow traffic from `197.211.52.179`.
- Updates `default-allow-rdp` rule on `nsg-windows-weu` to only allow traffic from `197.211.52.179`.

This prevents unauthorized scanning and brute-force attacks from malicious actors on the internet.

---

## Evidence

### SSH Connection to Linux VM
![SSH Connection to vm-linux-weu](screenshots/ssh_linux_connection.png)
*Successful SSH session — hostname `vm-linux-weu`, user `azureadmin`, Ubuntu 22.04 LTS*

### VM Inventory (Azure Portal)
![Azure Portal VM List](screenshots/vm_list_portal.png)
*Both VMs running in West Europe on Standard_B2ts_v2*

### Linux NSG — SSH Rule Restricted to Admin IP
![Linux NSG SSH Rule](screenshots/nsg_linux_ssh_rule.png)
*Port 22 restricted to 197.211.52.179 only*

### Windows NSG — RDP Rule Restricted to Admin IP
![Windows NSG RDP Rule](screenshots/nsg_windows_rdp_rule.png)
*Port 3389 restricted to 197.211.52.179 only*

### RDP Connection to Windows VM
![RDP Connection to vm-windows-weu](screenshots/rdp_windows_connection.png)
*Successful RDP session to `vm-windows-weu` at `20.229.20.124` using credentials `azureadmin`*

---

## Project Files

| File | Description |
|------|-------------|
| `vm_setup.ps1` | Provisions the resource group, Linux VM, and Windows VM with open NSG rules for initial setup |
| `secure_nsg.ps1` | Locks down both NSGs to restrict SSH and RDP to admin IP only |
| `nsg_config.md` | Detailed NSG rule documentation and security rationale |
| `screenshots/` | Evidence screenshots of VMs, connections, and NSG configurations |


# NSG Security Configuration Report

## Overview
This document details the Network Security Group (NSG) rules configured for both Virtual Machines in this assignment. NSGs act as a cloud-based firewall, controlling inbound and outbound traffic at the network interface and subnet level.

---

## Resource Group
- **Name:** `rg-vm-access-weu`
- **Location:** West Europe

---

## Linux VM — NSG: `nsg-linux-weu`

### Inbound Security Rules

| Priority | Name               | Port | Protocol | Source IP          | Action |
|----------|--------------------|------|----------|--------------------|--------|
| 1000     | default-allow-ssh  | 22   | TCP      | 197.211.52.179    | Allow  |
| 65000    | AllowVnetInBound   | Any  | Any      | VirtualNetwork     | Allow  |
| 65001    | AllowAzureLoadBalancerInBound | Any | Any | AzureLoadBalancer | Allow |
| 65500    | DenyAllInBound     | Any  | Any      | *                  | Deny   |

### Security Rationale
The SSH rule (port 22) was initially open to `*` (any source) to allow initial connectivity. It was then locked down to a single specific source IP address (`197.211.52.179`) using the `secure_nsg.ps1` script. This restricts management access exclusively to the administrator's workstation, preventing brute-force attacks and unauthorized access from internet-facing scanners.

---

## Windows VM — NSG: `nsg-windows-weu`

### Inbound Security Rules

| Priority | Name    | Port | Protocol | Source IP       | Action |
|----------|---------|------|----------|-----------------|--------|
| 1000     | rdp     | 3389 | TCP      | 197.211.52.179 | Allow  |
| 65000    | AllowVnetInBound | Any | Any | VirtualNetwork | Allow |
| 65001    | AllowAzureLoadBalancerInBound | Any | Any | AzureLoadBalancer | Allow |
| 65500    | DenyAllInBound | Any | Any | * | Deny |

### Security Rationale
The RDP rule (port 3389) was similarly restricted from `*` to the administrator's specific IP (`197.211.52.179`). Remote Desktop Protocol is a common target for credential-stuffing attacks; restricting the source IP eliminates the vast majority of attack surface.

---

## Security Script

The `secure_nsg.ps1` PowerShell script automates the IP-restriction lockdown:

```powershell
# Restricts both NSGs to admin IP
az network nsg rule update --resource-group rg-vm-access-weu \
    --nsg-name "nsg-linux-weu" --name "default-allow-ssh" \
    --source-address-prefixes 197.211.52.179

az network nsg rule update --resource-group rg-vm-access-weu \
    --nsg-name "nsg-windows-weu" --name "rdp" \
    --source-address-prefixes 197.211.52.179
```

---

## Evidence Screenshots

| Screenshot | Description |
|------------|-------------|
| `screenshots/vm_list_portal.png` | Azure Portal showing both VMs running |
| `screenshots/nsg_linux_ssh_rule.png` | Linux NSG SSH rule with restricted source IP |
| `screenshots/nsg_windows_rdp_rule.png` | Windows NSG RDP rule with restricted source IP |
| `screenshots/ssh_linux_connection.png` | Successful SSH connection to vm-linux-weu |



# =============================================================================
# Secure NSG Script
# Restricts SSH and RDP access to the administrator's specific IP address
# =============================================================================

$RG_NAME     = "rg-vm-access-weu"
$ADMIN_IP    = "197.211.52.179"

Write-Host "============================================================" -ForegroundColor Cyan
Write-Host "  Securing Network Security Groups (NSGs)" -ForegroundColor Cyan
Write-Host "  Restricting access to Admin IP: $ADMIN_IP" -ForegroundColor Cyan
Write-Host "============================================================" -ForegroundColor Cyan

# ── 1. Secure Linux NSG (Port 22) ─────────────────────────────────────────────
Write-Host "Securing Linux VM (nsg-linux-weu)..." -ForegroundColor Yellow
az network nsg rule update `
    --resource-group $RG_NAME `
    --nsg-name "nsg-linux-weu" `
    --name "default-allow-ssh" `
    --source-address-prefixes $ADMIN_IP `
    --output none

Write-Host "  [OK] SSH access restricted to $ADMIN_IP" -ForegroundColor Green

# ── 2. Secure Windows NSG (Port 3389) ─────────────────────────────────────────
Write-Host "Securing Windows VM (nsg-windows-weu)..." -ForegroundColor Yellow
az network nsg rule update `
    --resource-group $RG_NAME `
    --nsg-name "nsg-windows-weu" `
    --name "rdp" `
    --source-address-prefixes $ADMIN_IP `
    --output none

Write-Host "  [OK] RDP access restricted to $ADMIN_IP" -ForegroundColor Green
Write-Host "============================================================" -ForegroundColor Cyan
Write-Host "  NSG Security Configuration Complete!" -ForegroundColor Cyan
Write-Host "============================================================" -ForegroundColor Cyan


# =============================================================================
# Azure VM Access and Networking Setup Script
# Assignment: Cloud VM Connectivity
# =============================================================================

$SUBSCRIPTION_ID = "9335a9cd-ae74-439b-94b3-d965ca478c53"
$LOCATION        = "westeurope"
$RG_NAME         = "rg-vm-access-weu"
$ADMIN_USER      = "azureadmin"
$ADMIN_PASS      = "Password123!@#"  # Note: Required for Windows VM

Write-Host "============================================================" -ForegroundColor Cyan
Write-Host "  Azure VM Infrastructure Setup" -ForegroundColor Cyan
Write-Host "============================================================" -ForegroundColor Cyan

# ── Set active subscription ───────────────────────────────────────────────────
az account set --subscription $SUBSCRIPTION_ID

# ── Create Resource Group ─────────────────────────────────────────────────────
Write-Host "[1/3] Creating Resource Group..." -ForegroundColor Yellow
az group create --name $RG_NAME --location $LOCATION --output none
Write-Host "  [OK] $RG_NAME created." -ForegroundColor Green

# ── Deploy Linux VM ───────────────────────────────────────────────────────────
Write-Host "[2/3] Deploying Linux VM (Ubuntu)... This will take a few minutes." -ForegroundColor Yellow
az vm create `
    --resource-group $RG_NAME `
    --name "vm-linux-weu" `
    --image "Ubuntu2204" `
    --admin-username $ADMIN_USER `
    --generate-ssh-keys `
    --public-ip-sku Standard `
    --nsg "nsg-linux-weu" `
    --size "Standard_B2ts_v2" `
    --output none
Write-Host "  [OK] Linux VM deployed." -ForegroundColor Green

# ── Deploy Windows VM ─────────────────────────────────────────────────────────
Write-Host "[3/3] Deploying Windows VM (Server 2022)... This will take a few minutes." -ForegroundColor Yellow
az vm create `
    --resource-group $RG_NAME `
    --name "vm-windows-weu" `
    --image "Win2022Datacenter" `
    --admin-username $ADMIN_USER `
    --admin-password $ADMIN_PASS `
    --public-ip-sku Standard `
    --nsg "nsg-windows-weu" `
    --size "Standard_B2ts_v2" `
    --output none
Write-Host "  [OK] Windows VM deployed." -ForegroundColor Green

Write-Host "============================================================" -ForegroundColor Cyan
Write-Host "  Setup Complete! Gathering Connection Details..." -ForegroundColor Cyan
Write-Host "============================================================" -ForegroundColor Cyan

$LINUX_IP = az vm show -d -g $RG_NAME -n "vm-linux-weu" --query publicIps -o tsv
$WIN_IP   = az vm show -d -g $RG_NAME -n "vm-windows-weu" --query publicIps -o tsv

Write-Host ""
Write-Host "--- CONNECTION DETAILS ---" -ForegroundColor Yellow
Write-Host "Linux VM Public IP: " -NoNewline; Write-Host $LINUX_IP -ForegroundColor White
Write-Host "  SSH Command:      ssh $ADMIN_USER@$LINUX_IP" -ForegroundColor White
Write-Host ""
Write-Host "Windows VM Public IP:" -NoNewline; Write-Host $WIN_IP -ForegroundColor White
Write-Host "  RDP Username:     $ADMIN_USER" -ForegroundColor White
Write-Host "  RDP Password:     $ADMIN_PASS" -ForegroundColor White
Write-Host "--------------------------" -ForegroundColor Yellow
Write-Host ""
