# Home Lab Active Directory - Full Simulation

This project consists of a **complete Active Directory laboratory**, implemented using virtual machines, with automated user provisioning and integrated network services, ideal for practicing system and network administration.

---

## Objective

- Simulate a real corporate AD environment.
- Automatically create and manage 1,000 users.
- Configure essential network services: DHCP, NAT/RAS, and VPN.
- Test Windows 10 client logins within the domain.
- Learn and document the AD infrastructure step by step.

---

## Environment Structure

- **Domain Controller (DC)**: Windows Server 2019
- **RAM**: 2GB
- **CPU**: 1 core
- **Disk**: 40GB
- **Network Adapters**:
  - Adapter 1: NAT (Internet)
  - Adapter 2: Internal Network
    - IP: 172.16.0.1
    - Subnet: 255.255.255.0
    - DNS: 127.0.0.1
- **Installed Services**: AD DS, DHCP, NAT/RAS
- **Clients**: Windows 10 VM (receives IP from the DC)

![Home Lab Architecture](images/ad-screenshot.png)

---

## Step-by-Step

### 1️⃣ Windows Server 2019 Installation and Network Configuration
- Create a VM with two network adapters:
  - **Internet NIC (_INTERNET_)** → NAT
  - **Internal NIC (_X_internal_X_)** → Manual IP 172.16.0.1  
  ![Network Adapters Screenshot](images/network_adapters.png)
  ![Internal Network Configuration Screenshot](images/config-internal-network.png)

---

### 2️⃣ Active Directory Configuration
- Install Active Directory Domain Services via Server Manager.
- Create a new **forest**: `mydomain.com`
- Restart the server.
- Create an **admin account** and OU `_ADMINS`:
  - User: `a-toliveira` (Password1)  
  ![AD Users & Computers - Admin](images/admin-account.png)
- Log in using the created admin account.

---

### 3️⃣ NAT / RAS Configuration
- Allow Windows 10 clients to access the Internet through the DC.
- Configuration path:
  - Server Manager → Remote Access → Routing and Remote Access → Network Address Translation (NAT)  
  ![RAS NAT Screenshot](images/ras-nat.png)

---

### 4️⃣ DHCP Configuration
- Configure a new scope:
  - IP range: 172.16.0.100 – 172.16.0.200
  - Gateway: 172.16.0.1
  - DNS: 127.0.0.1
  - Domain Name: `mydomain.com`  
  ![DHCP Scope Screenshot](images/dhcp-scope.png)

---

### 5️⃣ Creating 1,000 Users via PowerShell
- Script: `scripts/create-users.ps1`
- Default password: `Password1`
- Username format: first letter of the first name + last name (e.g., Tomas Oliveira → `toliveira`)
- Procedure:
  1. Read `name.txt` containing the list of names.
  2. Convert the password into a secure string object.
  3. Create the `_USERS` OU.
  4. Loop through the list and create each user in AD.  
  ![PowerShell Script Screenshot](images/powershell-script.png)  
  ![AD Users - 1000 Users](images/ad-users.png)

---

### 6️⃣ Windows 10 Client Configuration
- Windows 10 VM configured with an internal network adapter.
- Receives IP from the DC:
  - IP: 172.16.0.100
  - Subnet: 255.255.255.0
  - Gateway: 172.16.0.1  
  ![Windows 10 ipconfig Screenshot](images/client-ipconfig.png)
- Tests:
  - `ping www.google.com` → DNS working
  - `ping mydomain.com` → Domain resolution successful  
  ![Ping Test Screenshot](images/client-ping.png)
- Rename hostname to `CLIENT1` and join it to the domain.
- Verify in DHCP and AD:  
  ![DHCP Leases Screenshot](images/dhcp-leases.png)  
  ![AD Computers Screenshot](images/ad-computers.png)
- Test login with any created user (`aabrev`):
  ```powershell
  whoami
  # Result: mydomain\aabrev
