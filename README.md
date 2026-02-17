# Home Lab Active Directory - Simulação Completa

<<<<<<< HEAD
Este projeto consiste num **laboratório completo de Active Directory**, implementado em máquinas virtuais, com usuários automatizados e integração de serviços de rede, ideal para prática de administração de sistemas e redes.

---

## Objetivo

- Simular um ambiente corporativo AD real.
- Criar e gerir 1.000 usuários automaticamente.
- Configurar serviços de rede essenciais: DHCP, NAT/RAS e VPN.
- Testar login de clientes Windows 10 no domínio.
- Aprender e documentar passo a passo a infraestrutura AD.

---

## Estrutura do Ambiente

- **Domain Controller (DC)**: Windows Server 2019
- **RAM**: 2GB
- **CPU**: 1 core
- **Disco**: 40GB
- **Adaptadores de Rede**:
  - Adapter 1: NAT (Internet)
  - Adapter 2: Internal Network
    - IP: 172.16.0.1
    - Subnet: 255.255.255.0
    - DNS: 127.0.0.1
- **Serviços instalados**: AD DS, DHCP, NAT/RAS
- **Clientes**: Windows 10 VM (recebem IP do DC)

![Arquitetura do Home Lab](images/ad-screenshot.png)

---

## Passo a Passo

### 1️⃣ Instalação do Windows Server 2019 e configuração de rede
- Criar VM com dois adaptadores:
  - **Internet NIC (_INTERNET_)** → NAT
  - **Internal NIC (_X_internal_X_)** → IP manual 172.16.0.1  
  ![Print adaptadores de rede](images/Step1_Configure_Network_Adapters.png)
  ![Print Configuração de Rede Interna](images/Step1_Configure_Network_Adapters.png)
---

### 2️⃣ Configuração do Active Directory
- Instalar AD Domain Services via Server Manager.
- Criar novo **forest**: `mydomain.com`
- Reiniciar servidor.
- Criar **conta admin** e OU `_ADMINS`:
  - Usuário: `a-toliveira` (Password1)  
  ![Print AD Users & Computers - Admin](images/admin-account.png)
- Login com usuário admin criado.

---

### 3️⃣ Configuração de NAT / RAS
- Permitir que clientes Windows 10 acedam à Internet via DC.
- Configuração:
  - Server Manager → Remote Access → Routing and Remote Access → Network Address Translation (NAT)  
  ![Print RAS NAT](images/ras-nat.png)

---

### 4️⃣ Configuração de DHCP
- Configurar novo scope:
  - Faixa IP: 172.16.0.100 – 172.16.0.200
  - Gateway: 172.16.0.1
  - DNS: 127.0.0.1
  - Domain Name: `mydomain.com`  
  ![Print DHCP Scope](images/dhcp-scope.png)

---

### 5️⃣ Criação de 1.000 usuários via PowerShell
- Script: `scripts/create-users.ps1`
- Senha padrão: `Password1`
- Nome de usuário: primeira letra do primeiro nome + sobrenome (ex.: Tomas Oliveira → `toliveira`)
- Procedimento:
  1. Ler `name.txt` com nomes.
  2. Converter senha para objeto seguro.
  3. Criar OU `_USERS`.
  4. Loop para criar cada usuário no AD.  
  ![Print script PowerShell](images/powershell-script.png)  
  ![Print AD Users - 1000 users](images/ad-users.png)

---

### 6️⃣ Configuração do cliente Windows 10
- VM Windows 10 configurada com adaptador interno.
- Recebe IP do DC:
  - IP: 172.16.0.100
  - Subnet: 255.255.255.0
  - Gateway: 172.16.0.1  
  ![Print ipconfig Windows 10](images/client-ipconfig.png)
- Testes:
  - `ping www.google.com` → DNS funcionando
  - `ping mydomain.com` → Resolução do domínio  
  ![Print ping testes](images/client-ping.png)
- Renomear hostname para `CLIENT1` e associar ao domínio.
- Verificar no DHCP e AD:  
  ![Print DHCP leases](images/dhcp-leases.png)  
  ![Print AD Computers](images/ad-computers.png)
- Login de teste com qualquer usuário criado (`aabrev`):
  ```powershell
  whoami
  # Resultado: mydomain\aabrev
=======
A Home Lab setup simulating a full Active Directory environment, and a script w/ PowerShell to add 1,000 users and integrated network services.
>>>>>>> refs/remotes/origin/main

