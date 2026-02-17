# Home-Lab-running-AD

A Home Lab setup simulating a full Active Directory environment, with PowerShell scripts to create and manage 1,000 users and integrated network services.

## Images
![Home Lab architecture showing Active Directory, servers, network, and service integration](images/ad-screenshot.png)


## Steps

Step1 -> Install Windows Server 2019 - Create a VM Box (DC) 
        Configure the 2 adapters - Screenshoot
                    -> (Internet NIC (_INTERNET_) - default
                    -> Internal NIC (X_internal_X) - Screenshoot -> IP address assigned manually (172.16.0.1)))

       
Step2 -> Install AD Domain Services(In Server Manager):
         . Promote this server to a domain - Add new forest -> mydomain.com
         . Restar Computer and now in Login Page it appears MYDOMAIN\Administrator (default) -> Password1
         . Create our Admin Account (Active Directory Users and Computers)
            -> Create Organizational Unit for admins (_ADMINS)
            -> Create our Account and assigned2 it to _ADMINS (a-toliveira) - Screenshoot of admin account within group _ADMINS 
         . Login with that user admin (a-toliveira, Password1)

Step3 -> Install RAS/NAT: In Server Manager - Remote Access
        .Permite que os Win10 clients que tenha a sua PVN mas que consiga aceder á Internet através do DC.
        . Routing and Remote Access -> Network address translation (NAT) -> Select the public interface (_INTERNET_)   (Print)
        
Step4 -> Install a DHCP Server in DC
        .Configure DHCP -> Add new Scope, Configure range (172.16.0.100-200), Router (Default GateWay (172.16.0.1)) And DomainName and DNS Server.

        
        
Step5 -> Create and Run PowerShell script to create 1000 users 
        . The same password: Password1
        . Getcontent -> name.txt (1000 names) convert to array
        . Convert String password to password Object
        . Create Organizational Unit (_USERS)
        . Loop for the array of Names to create the username (takes first letter of First name and "glues" to last name and convert everything to lower case (Ex. Tomas Oliveira -> toliveira))
        . Create a user with his info (password, first and last name, username,)
        
        (Print before runnig the script in windows powershell)
        (Print no AD Users and Computers - _USERS-> Find -> 1000 users found)
        

Step6 -> Install Windows 10 and create the VM - (Should receive the IP from DC)
        . Config Adapter 1 -> (Internal network)
        . Open terminal - ipconfig -> IP address by DC (172.16.0.100), Subnet Mask and Default Gateway (172.16.0.1) - (Print)
        . Try ping www.google.com -> if google.com resolved this means that dns server is working (Print)
        . Try ping mydomain.com (Print)
        . Rename hostname to CLIENT1 and memberof mydomain.com (Print)
        
        
Step7:
        Verificar em DHCP -> Addresses leases o IP que foi assigned (172.16.0.100) 
        Verificar em Active Directory Users and Computers -> Computers -> CLIENT1  
        Fazer login com qualquer username (EX. aabrev) e password: Password1 (Print terminal whoami -> mydomain\aabrev)
       



## Domain Controller (DC) Configuration
- OS: Windows Server 2019
- RAM: 2GB
- CPU: 1 cores
- Disk: 40GB
- Network Adapters:
  - Adapter 1: NAT (Internet access)
  - Adapter 2: Internal Network
    - IP: 172.16.0.1
    - Subnet: 255.255.255.0
    - DNS: 127.0.0.1


