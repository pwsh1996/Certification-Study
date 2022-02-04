| Skills Measured | Percent of Exam |
| --- | --- |
| [Deploy and Manage Active Directory Domain Services (AD DS) in on-premises and cloud environments](#1) | 30-35% |
| [Manage Windows Servers and workloads in a hybrid environment](#2) | 10-15% |
| [Manage virtual machines and containers](#3) | 15-20% |
| [Implement and manage an on-premises and hybrid networking infrastructure](#4) | 15-20% |
| [Manage storage and file services](#5) | 15-20% |

🔳[52] Needs to be Studied
📚[17] Read the Docs
⏹[21] Did at Work
✅[11] Studied and did Hands-On Testing

# <a name="1"></a>Deploy and Manage Active Directory Domain Services (AD DS) in on-premises and cloud environments
## Deploy and manage AD DS domain controllers
### ✅ deploy and manage domain controllers on-premises
> **Active Directory Domain Services (AD DS)** - A searchable, hierarchical directory for user, group, and computer accounts

> **Domain Controller (DC)** - A Windows Server host that makes its AD DS database available to other machines in a controlled manner

| Protocol | Port |
| --- | --- |
| Lightweight Directory Access Protocol (LDAPv3) | TCP/UDP 389 |
| Domain Name Service (DNS) | TCP/UDP 53 |
| NT LAN Manager (NTLM) | TCP 139/UDP 138,139 |
| Kerberos | TCP/UDP 88 |

**Deploy**

> *Note:* All network interfaces on the DC should have a static IP for reliable DNS operation

Start with adding the role. This command installs the AD DS server role and installs the AD DS and AD LDS server administration tools
```powershell
Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools
```
Installing the feature **Active Directory Domain Services** installs the PowerShell module **ADDSDeployment**, You can see what all commands it has with 
```powershell
Get-Command -module ADDSDeployment
```
```
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Add-ADDSReadOnlyDomainControllerAccount            1.0.0.0    addsdeployment
Cmdlet          Install-ADDSDomain                                 1.0.0.0    addsdeployment
Cmdlet          Install-ADDSDomainController                       1.0.0.0    addsdeployment
Cmdlet          Install-ADDSForest                                 1.0.0.0    addsdeployment
Cmdlet          Test-ADDSDomainControllerInstallation              1.0.0.0    addsdeployment
Cmdlet          Test-ADDSDomainControllerUninstallation            1.0.0.0    addsdeployment
Cmdlet          Test-ADDSDomainInstallation                        1.0.0.0    addsdeployment
Cmdlet          Test-ADDSForestInstallation                        1.0.0.0    addsdeployment
Cmdlet          Test-ADDSReadOnlyDomainControllerAccountCreation   1.0.0.0    addsdeployment
Cmdlet          Uninstall-ADDSDomainController                     1.0.0.0    addsdeployment
```
For each Install, Add, and Uninstall command there is a Test command that takes the same arguments (excluding SkipPreChecks).

| Task | Permissions |
| --- | --- |
| **Install a new forest** | **Local Administrator** of the computer |
| **Install a new child domain or new domain tree** | **Enterprise Admin** |
| **Install an additional domain controller on an existing domain** | **Domain Admin** |

🌳**Installing a new forest root domain**
To install a new forest named recyberia.com and be securely prompted to provide the Directory Services Restore Mode (DSRM) password use
```powershell
Install-ADDSForest -DomainName "recyberia.com" -InstallDNS
```
If you want to be more granular
```powershell
Install-ADDSForest -DomainName "recyberia" -DomainNetbiosName "RECYBERIA" -DomainMode WinThreshold -ForestMode WinThreshold -LogPath "C:\Logs" -SysvolPath "C:\Logs\SYSVOL" -DatabasePath "C:\Databases\NTDS"
```

`-DomainNetbiosName` Specifies the NetBIOS name for the root domain in the new forest

`-DomainMode` Specifies the domain functional level 

The accepted values are
- Win2003 : *Windows Server 2003*
- Win2008 : *Windows Server 2008*
- Win2008R2 : *Windows Server 2008 R2* (Default)
- Win2012 : *Windows Server 2012*
- Win2012R2 : *Windows Server 2012 R2*
- WinThreshold : *Windows Server 2016*

The domain functional level cannot be lower than the forest functional level, but it can be higher

`-ForestMode` Specifies the forest functional level

The accepted values are
- Win2003 : *Windows Server 2003* (Default when installing on Windows Server 2008 R2)
- Win2008 : *Windows Server 2008*
- Win2008R2 : *Windows Server 2008 R2* (Default)
- Win2012 : *Windows Server 2012*
- Win2012R2 : *Windows Server 2012 R2*
- WinThreshold : *Windows Server 2016*

> Note: Do not store the Active Directory database, log files, or SYSVOL folder on a data volume formatted with Resilient File System (ReFS)

`-LogPath` Specifies the fully quallified, non-UNC path to a directory where the log file for this operation is written

*%SYSTEMROOT%\NTDS* (Default)

`-SysvolPath` Specifies the fully quallified, non-UNC path to a directory where the Sysvol file is written

*%SYSTEMROOT%*\SYSVOL (Default)

`-DatabasePath` Specifies the fully quallified, non-UNC path to a directory that contains the domain database

*%SYSTEMROOT%\NTDS*

🌳**Installing an addtional (replica) domain controller**
To install a domain controller and DNS server in the recyberia.com domain and be prompted to supply the domain Administrator credentials and the DSRM password use
```powershell
Install-ADDSDomainController -Credential (Get-Credential) -DomainName "recyberia.com" -InstallDNS
```
If you want to be more granular
```powershell
Install-ADDSDomainController -Credential (Get-Credential) -DomainName "recyberia.com" -InstallDNS -DatabasePath "C:\Windows\NTDS" -LogPath "C:\Windows\Logs" -SysvolPath "C:\Windows\SYSVOL" -ReplicationSourceDC "dco1.recyberia.com" -NoGlobalCatalog
```

`-ReplicationSourceDC` Specifies the name of the domain controller to be used as the source for replicating to this domain controller.

`-NoGlobalCatalog` Indicates that the DC will not be a global catalog server. By default the domain controller that you are installing is a global catalog server.


🌳**Installing a new child or tree domain**
To create a new child domain named test.recyberia.com use
```powershell
Install-ADDSDomain -Credential (Get-Credential) -NewDomainName "test" -ParentDomainName "recyberia.com" -InstallDNS -CreateDNSDelegation -DomainMode WinThreshold
```

Logs can be found at
- %systemroot%\debug\dcpromo.log
- %systemroot%\debug\dcpromoui.log

*Resources:* <br />
[Install Active Directory Domain Services (Level 100) | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/install-active-directory-domain-services--level-100-) <br />
[Install a New Windows Server 2012 Active Directory Forest (Level 200) | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/install-a-new-windows-server-2012-active-directory-forest--level-200-) <br />
[What's New in Active Directory Domain Services INstallation and Removal | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/what-s-new-in-active-directory-domain-services-installation-and-removal) <br />
[Install-ADDSForest | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/addsdeployment/install-addsforest?view=windowsserver2022-ps) <br />
[Active Directory Domain Services Deep Dive | John Savill YouTube](https://www.youtube.com/watch?v=4qC7H-y7oKI)
### 📚 deploy and manage domain controllers in Azure
> Note: The VMs that are DCs should have a seperate data disk for storing the database, logs, and sysvol folder for Active Directory. Do not store these items on the same disk as the operating system. By default data disks that are attached to a VM use write-through caching. However this form of caching can conflict with the requirements of AD DS. For this reason, set the *Host Cache Preference* setting on the data disk to *None*.

Deploy at least two VMs as DCs and add them to an availability set.

The DCs in Azure should be part of a seperate AD DS site. Make a site link from the Azure site, to your on-premises AD DS sites, and AD DS will automatically perform the most efficient database replication possible.

It's not recommended to deploy FSMO roles on the DCs in Azure

Azure VMs that are operating as DCs should be shutdown from inside the guest OS and not through the Azure portal.

*Resources:* <br />
[Deploy AD DS in an Azure virtual network | Microsoft Docs](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/identity/adds-extend-domain)
### 📚 deploy Read-Only Domain Controllers (RODCs)
*Resources:* <br />
[Install a Windows Server 2012 Active Directory Read-Only Domain Controller (RODC) (Level 200) | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/rodc/install-a-windows-server-2012-active-directory-read-only-domain-controller--rodc---level-200-)
### ✅ troubleshoot flexible single master operations (FSMO) roles
**Multi-master model** A multi-master enabled database, like AD, provides the flexibility of allowing changes to occur at any DC in the enterprise. But it also introduces the possibility of conflicts that can potentially lead to problems once the data is replicated. For certain types of changes, Windows incorporates methods to prevent conflicting Active Directory updates from occurring.

**Single-master model** To prevent conflicting updates in Windows, the Active Directory performs updates to certain objects in a single-master fashion. In a single-master model, only one DC in the entire directory is allowed to process updates. Active Directory extends the single-master model found in earlier versions of Windows to include multiple rols, and the ability to transfer roles to any DC in the enterprise. Because an AD role isn't bound to a single DC, it's referred to as an FSMO role.

FSMO roles:
- Schema master
- Domain naming master
- RID master
- PDC emulator
- Infrastructure master

📜**Schema master FSMO role** 
`only one schema master per forest`
The schema master is the DC responsible for performing updates to the directory schema, that is, the *schema* naming context or `LDAP://cn=schema,cn=configuration,dc=<domain>`. This DC is the only one that can process updates to the directory schema. Once the schema update is complete, it's replicated from the schema master to all other DCs in the directory.

📜**Domain naming master FSMO role**
`only one domain naming master per forest`
The domain naming master is the DC responsible for making changes to the forest-wide domain name space of the directory, that is, the *Partitions\Configuration* naming context or `LDAP://CN=Partitions,CN=Configuration,DC=<domain>`. This DC is the only one that can add or remove a domain from the directory. It can also add or remove cross references to domains in external directories.
  
📜**RID master FSMO role**
`only one rid master per domain`
The rid master is the single DC responsible from processing RID Pool requests from all DCs within the domain. It's also responsible for removing an object from its domain and putting it in another domain during an object move. <br />
When a DC creates a security principal object, like a user or group, it attaches a unique Security ID (SID) to the object. This SID consists of:
- a domain SID that's the same for all SIDs created in a domain
- a relative ID (RID) that's unique for each security Principal SID created in a domain <br />
Each DC in a domain is allocated a pool of RIDs that it can use on new security principals it creates. WHen the DC's RID Pool falls below a threshold, it requests more from the RID master
  
📜**PDC emulator FSMO role**
`only one pdc emulator per domain`
the pdc emulator is necessary to synchronize time in an enterprise. Windows includes the W32Time (Windows Time) service that is required by Kerberos. All Windows-based computers within an enterprise use a common time, to ensure the use of a hierarchical relationship that controls authority. <br />
The pdc emulator of a domain is authoritave for the domain. The PDC emulator at the root of the forest becomes authoritative for the enterprise, and shuld be configured to gather the time from an external source. <br />
The pdc emulator role holder retains the following functions:
- Password changes done by other DCs are replicated preferentially to the pdc emulator
- When authentication failures occur on a DC because of an incorrect password, the failures are forwared to the pdc emulator before a bad password failure message is given to the user
- Account lockout is processed on the pdc emulator
- The pdc emulator performs all the functions that a Windows NT4.0 server-based pdc would perform
  
📜**Infrasturcture master FSMO role**
`only one infrasturcture master per domain`
The infrastructure master is the DC responsible for updating an object's SID and distinguished name (DN) in a cross-domain object reference. When an object in one domain is referenced by another object in another domain, it represents the reference by : the GUID, the SID (references to security principals), and the DN on the object being referenced.
> Note: The Infrastructure Master role should be held by a DC that's not a Glocal Gatalog (GC). Unless all DCs are GCs then it isn't important which DC holds the role

> Note: When the Recycle Bin optional feature is enabled, every DC is responsible to update its cross-domain object references. in which case there are no tasks associated with the Inrastructure Master and it isn't important which DC holds the role

To view what DCs hold what roles, go to Active Directory Users and Computers (*dsa.msc*), right-click on the domain object and select **Operations Masters** <br />
![image](https://user-images.githubusercontent.com/51274282/152245699-2ab9905d-4b40-4194-94bd-a59650a24347.png)

For the schema master add the Active Directory Schema snap-in to mmc, then right-click on "Active Directory Schema" and select Operations Masters

![image](https://user-images.githubusercontent.com/51274282/152247091-8870f54a-e6a0-4d47-bbe3-144810e774c7.png)

> Note: for the Active Directory Schema snap-in to be avalible, you may have to run `regsvr32 schmmgmt.dll` which should output something like "DllRegisterServer in schmmgmt.dll succeeded."

For the domain naming role, go to Active Directory Domains and Trusts and right-click Active Directory Domains and Trusts and select Operationas Manster.

![image](https://user-images.githubusercontent.com/51274282/152247432-a1f68eeb-ae45-43c5-b3e6-f6cf0fd8c1d6.png)

You can also run this to get the role holders in a domain
```
DCdiag /test:KNowsofroleholders /v
```

To transfer FSMO roles
```powershell
Move-ADDirectoryServerOperationMasterRole -Identity "dc02" -OperationMasterRole InfrastructureMaster
```

*Resources:* <br />
[Active Directory FSMO roles in Windows | Microsoft Docs](https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/fsmo-roles) <br />
[Transfer or seize FSMO roles in Active Directory Domain Services | Microsoft Docs](https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/transfer-or-seize-fsmo-roles-in-ad-ds) <br />
[Move-ADDirectoryServerOperationMasterRole | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/activedirectory/move-addirectoryserveroperationmasterrole?view=windowsserver2022-ps)
## Configure and manage multi-site, multi-domain, and multi-forest environments
### 📚 configure and manage forest and domain trusts
*Resources:* <br />
[Tutorial: Create an outbound forest trust to an on-premises domain in Azure Active Directory Domain Services | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/tutorial-create-forest-trust) <br />
[Managin Trusts | Microsoft Docs](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771568(v=ws.10))
### 📚 configure and manage AD DS sites

AD DS uses site information for many purposes, including routing replication, client affinity, system volume (SYSVOL) replication, Distributed File System Namesspaces (DFSN), and service location.

*Resources:* <br />
[Understanding Active Directory Site Topology | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/understanding-active-directory-site-topology)
### 📚 configure and manage AD DS replication
*Resources:* <br />
[Active Directory Replication Concepts | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/replication/active-directory-replication-concepts)
## Create and manage AD DS security principals
### ⏹ create and manage AD DS users and groups
*Resources:* <br />
[Managing Users | Microsoft Docs](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc754661(v=ws.10)) <br />
[Managing Groups | Microsoft Docs](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771069(v=ws.10))
### 📚 manage users and groups in multi-domain and multi-forest scenarios

Types of group accounts
- domain local groups - apply to the domain only and should be used for permission type groups
- global groups - apply to the domain only, and should be used for role type groups
- universal groups - apply to the forest, and should be used as big containers

*Resources:* <br />
[Understanding Group Accounts | Microsoft Docs](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc733001(v=ws.10))
### 📚 implement group managed service accounts (gMSAs)

There are two types of Managed Service Account, standalone Manged Service Account (sMSA) and group Managed Service Acount (gMSA)

> Note: The schema in the domain you are using gMSAs must be at a Server 2012 functional level

Microsoft Key Distribution Service (kdssvc.dll). There needs to be a master root key for Active Directory. To see if there is a key already, use
```powershell
Get-KdsRootKey
```
To make a new key
```powershell
Add-KdsRootKey -EffectiveImmediately
```

Failover clusters don't support gMSAs

AES should always be used for MSAs

> Note: The password change interval can only be set during creation. To change you must create a new gMSA.

*Resources:* <br />
[Group Managed Service Accounts Overview | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
### ⏹ join Windows Server to AD DS, Azure AD DS, and Azure AD
*Resources:* <br />
[Join a Computer to a Domain | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/join-a-computer-to-a-domain) <br />
[Tutorial: Join a Windows Server virtual machine to an Azure Active Directory Domain Services managed domain | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/join-windows-vm)
## Impliment and manage hybrid identities
### ⏹ implement Azure AD Connect

Changes are synced every 30 minutes

*Resources:* <br />
[Introduction to Azure AD Connect V2.0 | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-azure-ad-connect-v2)
### ⏹ manage Azure AD Connect Synchrinization
*Resources:* <br />
[Azure AD Connect sync: Understanding the architecture | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/concept-azure-ad-connect-sync-architecture)
### 📚 implement Azure AD Connect cloud sync

Changes are synced every 2 minutes

*Resources:* <br />
[What is Azure AD Connect cloud sync | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/cloud-sync/what-is-cloud-sync)
### 🔳 integrate Azure AD, AD DS, and Azure AD DS
*Resources:* <br />
[Integrate on-premises AD domains with Azure AD | Microsoft Docs](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/identity/azure-ad)
### ⏹ manage Azure AD DS
*Resources:* <br />
[Tutorial: Create and configure an Azure Active Directory Domain Services managed domain | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/tutorial-create-instance)
### ⏹ manage Azure AD Connect Health
*Resources:* <br />
[What is Azure AD Connect? | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-azure-ad-connect)
### 🔳 manage authentication in on-premises and hybrid enviroments
*Resources:* <br/>
[Choose the right authentication method for your Azure Active Directory hybrid identity solution | Microosft Docs](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/choose-ad-authn)
### 📚 configure and manage AD DS passwords
*Resources:* <br />
[Password and account lockout policies on Azure Active Directory Domain Services managed domains | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/password-policy)
## Manage Windows Server by using  domain-based Group Policies
### ⏹ implement Group Policy in AD DS
*Resources:* <br />
[Group Policy Overview | Microsoft Docs](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831791(v=ws.11))
### 🔳 implement Group Policy Preferences in AD DS
*Resources:* <br />
[About Group Policy Preferences | Microsoft Docs](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/policy/group-policy-preferences)
### 📚 implement Group Policy in Azure AD DS

In a hybrid environment, group policies configured in an on-premises AD DS environment aren't syncronized to Azure AD DS. To define configuration settings for users or computers in Azure AD DS, edit one of the default GPOs or create a custom GPO.

*Resources:* <br />
[Administer Group Policy in an Active Directory Domain Services manged domain | Microsoft Docs](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/manage-group-policy)
# <a name="2"></a>Manage Windows Servers and workloads in a hybrid environment
## Manage Windows Servers in a hybrid environment
### ⏹ deploy a Windows Admin Center gateway server
*Resources:*

https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/azure/deploy-wac-in-azure
### 🔳 configure a target machine for Windows Admin Center
*Resources:*

https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/azure/manage-vm
### ⏹ configure PowerShell Remoting
*Resources:*

https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/running-remote-commands?view=powershell-7.2&viewFallbackFrom=powershell-7.1%3FWT.mc_id%3Dmodinfra-39512-orthomas

https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/powershell-remoting-faq?view=powershell-7.2&viewFallbackFrom=powershell-7.1%3FWT.mc_id%3Dmodinfra-39512-orthomas
### 🔳 configure CredSSP or Kerberos delegation for second hop remoting

> The **Second Hop Problem** occurs when you remote into a server and enter a command on the server that attemps to access a resource on another server. The result would be that you are denied because the credentials you used to connect in the first place are not passed after that first connection.

**CredSSP** - balances ease of use and security

**Resource-based Kerberos constrained delegation** - higher security with simpler configuration

**Kerberos constrained delegation** - high security but requires domain administrator

*Resources:* <br />
[Making the second hop in PowerShell Remoting | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/ps-remoting-second-hop?view=powershell-7.2&viewFallbackFrom=powershell-7.1%3FWT.mc_id%3Dmodinfra-39512-orthomas) <br />
[Enable PowerShell "Second-Hop" Functionality with CredSSP | Microsoft Devblogs](https://devblogs.microsoft.com/scripting/enable-powershell-second-hop-functionality-with-credssp/)
### 📚 configure JEA for Powershell Remoting

JEA uses the principle of *Least Privilege*

*Resources:* <br />
[Just Enough Administration | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/jea/overview?view=powershell-7.2&viewFallbackFrom=powershell-7.1%3FWT.mc_id%3Dmodinfra-39512-orthomas) <br />
[PowerShell ♥ the Blue Team | Microsoft Devblogs](https://devblogs.microsoft.com/powershell/powershell-the-blue-team/)
## Manage Windows Servers and workloads by using Azure services
### 🔳 manage Windows Servers by using Azure Arc
*Resources:*

https://docs.microsoft.com/en-us/azure/azure-arc/servers/onboard-windows-admin-center
### 🔳 assign Azure Policy Guest Configure
*Resources:*

https://docs.microsoft.com/en-us/azure/azure-arc/servers/learn/tutorial-assign-policy-portal
### 🔳 deploy Azure services using Azure Virtual Machine on non-Azure machines
*Resources:*

https://docs.microsoft.com/en-us/azure/azure-arc/servers/learn/tutorial-enable-vm-insights
### 🔳 manage updates for Windows machines
*Resources:*

https://docs.microsoft.com/en-us/azure/automation/update-management/overview
### 📚 integrate Windows Servers with Log Analytics
The Azure Log Analytics agent collects telemetry from Windows and Linux virtual machines in any cloud, on-premises machines, and those monitored by System Center Operations Manager and sends collected data to you Log Analytics workspace in Azure Monitor.

> The Log Analytics agent is often referred to as the Microsoft Monitoring Agent (MMA).

*Resources:* <br />
[Log Analytics agent overview | Microsoft Docs](https://docs.microsoft.com/en-us/azure/azure-monitor/agents/log-analytics-agent) <br />
[Log Analytics virtual machine extension for Windows | Microsoft Docs](https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/oms-windows)
### 🔳 integrate Windows Servers with Azure Security Center
*Resources:*

https://docs.microsoft.com/en-us/azure/architecture/hybrid/hybrid-security-monitoring
### 🔳 manage IaaS virutal machines (VMs) in Azure that run Windows Server

*Resources:*

https://docs.microsoft.com/en-us/azure/virtual-machines/maintenance-and-updates
### 🔳 implement Azure Automation for hybrid workloads
*Resources:*

https://docs.microsoft.com/en-us/azure/architecture/hybrid/azure-automation-hybrid
### 🔳 create runbooks to automate tasks on target VMs
*Resources:*

https://docs.microsoft.com/en-us/azure/automation/automation-runbook-execution
### 🔳 implement DSC to prevent configuration drift in IaaS machines
*Resources:*

https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/dsc-overview

https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/dsc-windows

https://docs.microsoft.com/en-us/azure/virtual-machines/extensions/features-windows
# <a name="3"></a>Manage virtual machines and containers
## Manage Hyper-V and guest virtual machines
### ✅ enable VM enhanced session mode
> **Virtual Machine Connection (VMConnect)** lets you use a computer's local resources in a virtual machine, like removable USB flash drive or a printer. Enhanced session mode also lets you resize the VMConnect window.

![image](https://user-images.githubusercontent.com/51274282/152070460-c501629c-a57a-4af4-84cd-04eaee0cd1d3.png)

When Applied there is a button to switch to enhanced session mode at the top of VMConnect

![image](https://user-images.githubusercontent.com/51274282/152070762-82c19eb1-8984-4cfa-ac98-e881e9d7c751.png)

If you need to edit your connection settings you can run 

```powershell
VMConnect.exe <ServerName> <VMName> /edit
```
Requirements for using local resources
- The Hyper-V host must have **Enhanced session mode policy** and **Enhanced session mode** settings turned on.
- The computer on which you use VMConnect must run Windows 10, Windows 8.1, Windows Server 2016, or Windows Server 2012 R2.
- The virtual machine must have Remote Desktop Services enabled, and run Windows 10, Windows 8.1, Windows Server 2016, or Windows Server 2012 R2 as the guest operating system. 

Other Benifits
> **Prevent a VMConnect user from taking over another user's VMConnect session** - Not having enhanced session mode turned on may pose a security and privacy risk. If a user is connected and logged on to a virtual machine through VMConnect and another authorized user connects to the same virtual machine, the session will be taken over by the second user and the first user will lose the session. The second user will be able to view the first user's desktop, documents, and applications.

*Resources:* <br />
[Use local resources on Hyper-V virtual machine with VMConnect | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/learn-more/use-local-resources-on-hyper-v-virtual-machine-with-vmconnect) <br />
[Hyper-V Virtual Machine Connection | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/learn-more/hyper-v-virtual-machine-connect)
### ✅ manage VM using PowerShell Remoting, PowerShell Direct, and HVC.exe

Powershell Direct works on Windows 10 and Windows Server 2016 and above (OS requirment for the host and guest). PowerShell Direct allows Windows Powershell management inside a VM regardless of the network configuration or remote management settings on the host or guest VM.

You can either use **PSSession** or **Invoke-Command** to use it, no configuration required.

```powershell
Enter-PSSession -VMName "Win01"
```
or
```powershell
Invoke-Command -VMName "Win01" -ScriptBlock {Get-Process}
```

*Resources:* <br />
[Manage Windows virtual machines using PowerShell Direct | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/manage-windows-virtual-machines-with-powershell-direct) <br />
[Manage Hyper-V VMs using Powershell Direct | Microsoft ITOps Talk Blog](https://techcommunity.microsoft.com/t5/itops-talk-blog/manage-hyper-v-vms-using-powershell-direct/bc-p/1531743/highlight/true)
### ✅ configure nested virtualization
> Note: Nested Virtualization is supported in both Azure and on-premises. However, the nested virtual machines are not supported for production purposes. Labs, testing environments, demo environments, etc, are more of it's purpose.

**Intel CPUs with VT-x and EPT**
- (Host) Windows Server 2016 / Windows 10 or greater

**AMD EPYC/Ryzen CPUs**
- (Host) Windows Server 2022 / Windows 11 or greater

While the VM is off run 
```powershell
Set-VMProcessor -VMName "Win01" -ExposeVirtualizationExtensions $true
```
> Note: When Hyper-V is running inside a VM, the VM must be turned off to adjust it's memory. Meaning that even if dynamic memory is enabled, the ammount of memory will not fluctuate. Any attempt to adjust the ammount of memory while it's on will fail.

![image](https://user-images.githubusercontent.com/51274282/152213325-8750e004-9f48-43b6-9be9-f7277b811dc2.png)

**Networking**
- **Mac Address Spoofing** In order for packets to be routed throu two virtual switches, MAC address spoofing must be enabled on the first (L1) level of virtual switch. You can do it with 
```powershell
Set-VMNetworkAdapter -VMName "Win01" -MacAddressSpoofing On
```

- **Network Address Translation (NAT)** This is best suited for cases where MAC address spoofing is not possible, like in a public cloud environment. First a virtual NAT switch must be created in the host VM (the "middle" VM) 
```powershell
New-VMSwitch -Name VmNAT -SwitchType Internal
New-NetNat -Name LocalNAT -InternalIPInterfaceAddressPrefix "192.168.100.0/24"
# Then assign an IP to the net adapter
Get-NetAdapter "vEthernet (VmNat)" | New-NetIPAddress -IPAddress 192.168.100.1 -AddressFamily IPv4 -PrefixLength 24
# Then in each nested VM assign an IP address on that network
```

> Note: In my personal experience I did not have to do either and the networking just worked ㄟ( ▔, ▔ )ㄏ

*Resources:* <br />
[Run Hyper-V in a Virtual Machine with Nested Virtualization | Microsoft Docs](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/user-guide/nested-virtualization)
### ✅ configure VM memory

![image](https://user-images.githubusercontent.com/51274282/151913437-dfee810b-7999-416a-9d5c-c2e9e412ce3b.png)

Powershell:
```powershell
Get-VMMemory | fl -Property *
```
```powershell
Set-VMMemory -VMName <client01> -Buffer 20 -Priority 37 -MaximumBytes 5GB
```
You can also adjust the settings with, but it doesn't have the buffer or priority
```powershell
Get-VM -Name <client01> | Set-VM -MemoryMinimum 2GB
```

> **Dynamic Memory** - If you have idle or low-load virtual machines, as in pooled Virtual Desktop Infrastructure (VDI) environments, Dynamic Memory enables you to increase consolidation and improve reliability for restart operations. You also gain agility in responding to requirement changes with these new capabilities.

Hyper-V enables users to make the following configuration changes to Dynamic Memory when the virtual machine is running:
- Increase the maximum memory.
- Decrease the minimum memory.

> **Smart Paging** - Although minimum memory increases virtual machine consolidation numbers, it also brings a challenge. If a virtual machine has a smaller amount of memory than its startup memory and if it is restarted, Hyper-V needs additional memory to restart the virtual machine. Due to host memory pressure or virtual machine states, Hyper-V may not always have additional memory available. This can cause sporadic virtual machine restart failures. Smart Paging is used to bridge the memory gap between minimum memory and startup memory, and allow virtual machines to restart reliably.

To minimize the performance impact of Smart Paging, Hyper-V uses it only when all of the following occurs:
- The virtual machine is being restarted.
- There is no available physical memory.
- No memory can be reclaimed from other virtual machines running on the host.

Smart Paging is not used when:
- A virtual machine is being started from an “off state” (instead of a restart).
- Oversubscribing memory for a running virtual machine is required.
- A virtual machine is failing over in Hyper-V clusters.

Also note the following about how Smart Paging is used:
- Smart Paging files are created only when needed for a virtual machine.
- After the additional amount of memory is removed, Smart Paging files are deleted.
- Smart Paging is not used for this virtual machine again until another restart occurs and there is not enough physical memory.

| Setting | Description |
| --- | --- |
| Startup RAM	| Specifies the amount of memory required to start the virtual machine. The value needs to be high enough to allow the guest operating system to start, but should be as low as possible to allow for optimal memory utilization and potentially higher consolidation ratios. |
| Minimum RAM	| Specifies the minimum amount of memory that should be allocated to the virtual machine after the virtual machine has started. The value can be set as low as 32 MB to a maximum value equal tothe Startup RAM Value | 
| Maximum RAM |	Specifies the maximum amount of memory that this virtual machine is allowed to use. The value can be set from as low as the value for Startup RAM to as high as 1 TB. However, a virtual machine can use only as much memory as the maximum amount supported by the guest operating system. For example, if you specify 64 GB for a virtual machine running a guest operating system that supports a maximum of 32 GB, the virtual machine cannot use more than 32 GB. |
| Memory buffer |	Specifies how much memory Hyper-V will attempt to assign to the virtual machine compared to the amount of memory actually needed by the applications and services running inside the virtual machine. Memory buffer is specified as a percentage because the actual amount of memory that represents the buffer changes in response to changes in memory usage while the virtual machine is running. Hyper-V uses performance counters in the virtual machine that identify committed memory to determine the current memory requirements of the virtual machine and then calculates the amount of memory to add as a buffer. The buffer is determined using the following formula: <br /> <br /> Amount of memory buffer = how much memory the virtual machine actually needs / (memory buffer value / 100). <br /> <br /> For example, if the memory committed to the guest operating system is 1000 MB and the memory buffer is 20%, Hyper-V will attempt to allocate an additional 20% (200 MB) for a total of 1200 MB of physical memory allocated to the virtual machine. Note: The buffer is not maintained when there is not enough physical memory available in the computer to give every virtual machine its requested memory buffer. |
| Memory weight |	Provides Hyper-V with a way to determine how to distribute memory among virtual machines if there is not enough physical memory available in the computer to give every virtual machine its requested amount of memory. |

The current amount of memory available to virtual machines can be viewed in the following Performance Monitor counter, **Hyper-V Dynamic Memory Balancer – Available Memory**.
![image](https://user-images.githubusercontent.com/51274282/152041042-66f04885-e7db-4890-aa6f-865d0be4e582.png)

*Resources:* <br />
[Hyper-V Dynamic Memory Overview | Microsoft Docs](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831766(v=ws.11)) <br />
[Get-VMMemory | Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/hyper-v/get-vmmemory)
### ✅ configure Integrated Services
Integration services (also called integration components), are services that allow the virtual machine to communicate with the Hyper-V host. Many of these services are conveniences while others can be quite important to the VM's ability to function

They can be turned on and off in Hyper-V Manager or in PowerShell

![image](https://user-images.githubusercontent.com/51274282/152277044-cf4d4e90-f8ae-413e-a35e-1fe785c413c8.png)

```powershell
Get-VMIntegrationService -VMName "Win01"
```
```powershell
Enable-VMIntegrationService -VMName "Win01" -Name "Guest Service Interface"
# You can also use Disable-VMIntegrationService to disable it
```
They are also managable from the guest, although it's best to do it from the host

| Powershell Name | Windows Service Name | Description | Impact if disabled |
| --- | --- | --- | --- |
| Guest Service Interface | vmicguestinterface | Provides an interface for the Hyper-V to copy files from the VM | Low |
| Heartbeat | vmicheartbeat  | Reports that the virtual machine is running correctly | Varies |
| Key-Value Pair Exchange | vmickvpexchange | Provides a way to exchange basic metadata between the VM and host | Varies |
| Shutdown | vmicshutdown | Allows the host to trigger VMs shutdown | High |
| Time Synchronization | vmictimesync | Synchronizes the VM's clock with the host's clock | High |
| VSS | vmicvss | Allows Volume Shadow Copy to back up the VM without shutting it down | Varies |
|  | vmiccmsession | Provides a way to manage VMs with PowerShell without a network connection | Low |

*Resources:* <br />
[Manage Hyper-V Integration Services | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) <br />
[Hyper-V Integration Services | Microsoft Docs](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/reference/integration-services)
### 🔳 configure Discrete Device Assignment
*Resources:*

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/deploy/deploying-graphics-devices-using-dda

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/deploy/deploying-storage-devices-using-dda
### 📚 configure VM Resource Groups
*Resources:* <br />
[Hyper-V Host CPU Resource Management | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/manage-hyper-v-minroot-2016)
### 🔳 configure VM CPU Groups
*Resources:* <br />
[Virtual Machine Resource Controls | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/manage-hyper-v-cpugroups)
### ✅ configure hypervisor scheduling types
There are different virtual processor scheduling modes that determine how the hypervisor allocaates and manages work across guest virtual processors. You can select hypervisor scheduler types that are best suited for the guest VMs and configure the VMs to take advantage of the scheduling logic 

> Simultaneous multithreading (SMT) - A technique utilized in modern processor designs that allow the processor's resources to be shared by seperate, independent execution threads. The individual technologies are Intel HyperThreading and AMD SMT

Core Hyper-V concepts to understand
- Hyper-V creates and manages **VM partitions**, across which compute resources are allowcated and shared, under control of the hypervisor. **Partitions** provide strong isolation boundaries between all guest VMs, and between guest VMs and the root partition.
- The root partition is itself a VM partition, although it has unique properties and much greater privileges than guest VMs. The root partition provides the management services that control all guest VMs, provides virtual device support for guests, and manages all device I/O for guest VMs. Microsoft strongly recommends not runnig any application workloads in the root partition.
- Each virtual processor (VP) of the root partition is mapped 1:1 to an underlying logical processor (LP). A host VP always runs on the same underlying LP - there is no migration of the root partition's VPs.
- By default, the LPs on which host VPs run can also run guest VPs.
- A guest VP may be scheduled by the hypervisor to run on any available logical processor. While the hypervisor scheduler takes care to consider temporal cache locality, NUMA topology, and many other factors when scheduling a guest VP, ultimately the VP could be scheduled on any host LP.

**Hypervisor scheduler types**

🕓 The **Classic Scheduler** <br />
The classic scheduler provides a fair share, preemptive round-robin scheduling model for guest VPs.

Classic is the default on Windows Server 2016 and earlier

🕗 The **Core Scheduler**
The core scheduler offers a strong security boundary for guest workload isolation, and reduced performance variability for workloads inside of VMs that are running on an SMT-enabled virtualization host. The scheduler can iptionally expose SMT pairs to guest VMs. <br />
The overall result of the core scheduler is that:
- Guest VPs are contrained to run on underlying physicalcore pairs, isolating a VM to processor core boundaries, thus reducing vulnerability to side-channel snooping attacks from malicious VMs.
- Variability in throughput is significantly reduced.
- Performance is potentially reduced, because if only one of a group of VPs can run, only one of the instruction streams in the core executes while the other is left idle.
- The OS and appllications running in the guest VM can utilize SMT

Core is the defualt Windows Server 2019 on onward

🕥 The **Root Scheduler**

The root scheduler lets the hypervisor cede control of work scheduling to the root partition . The NT scheduler in theroot partition's OS instance manages all aspects of scheduling work to system LPs. It enables things like Windows Defender Application Guard (WDAG) on the host OS.

Root is default on Windows 10 version 1803 on onward (should not be used in Windows Server)

🚧**Managing the Scheduler**🚧

To set it on the guest VM use
```powershell
Set-VMProcessor -VMName "Win01" -HwThreadCountPerCore 0
```
> Note: Setting -HwThreadCountPerCore = 0 means it will use what the host is set to, you can also use 1 or 2 

To set it on the hostuse 
```
bcdedit /set hypervisorschedulertype Core
```
You can use Classic, Core, or Root

To see what the current scheduler is, there is an event log with the Event ID=2 that show the type.
- 0x1 - Classic Scheduler, SMT disabled
- 0x2 - Classic Scheduler
- 0x3 - Core Scheduler
- 0x4 - Root Scheduler

![image](https://user-images.githubusercontent.com/51274282/152429491-bcc534df-109d-4ba3-89f0-9ae203485809.png)

*Resources:* <br />
[Managing Hyper-V hypervisor scheduler types | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types) <br />
[About Hyper-V hypervisor scheduler type selection | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/about-hyper-v-scheduler-type-selection)
### ✅ manage VM Checkpoints

There are two types of Hyper-V checkpoints
- **Production Checkpoints** which are "point in time"
- **Standard Checkpoints** which captures the state, data, and hardware configureation

![image](https://user-images.githubusercontent.com/51274282/152468280-bff9e130-cc2f-49a1-ac1e-2b779f815cf5.png)

*Resources:* <br />
[Choose between standard or production checkpoints in Hyper-V | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/choose-between-standard-or-production-checkpoints-in-hyper-v) <br />
[Enable or disable checkpoints in Hyper-V | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/enable-or-disable-checkpoints-in-hyper-v)
### ⏹ implement high availability for virtual machines
*Resources:*

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn743844(v=ws.11)
### 📚 manage VHD and VHDX files
*Resources:* <br />
[Hyper-V Virtual Hard Disk Format Overview | Microsoft Docs](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831446(v=ws.11)) <br />
[Configure a Shared Virtual Hard Disk | Microsoft Docs](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn282283(v=ws.11))
### ⏹ configure Hyper-V network adapter
*Resources:*

https://docs.microsoft.com/en-us/azure-stack/hci/concepts/host-network-requirements
### ⏹ configure NIC teaming
*Resources:*

https://docs.microsoft.com/en-us/azure-stack/hci/concepts/host-network-requirements

https://docs.microsoft.com/en-us/windows-server/networking/technologies/nic-teaming/nic-teaming
### ⏹ configure Hyper-V switch
*Resources:*

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v-virtual-switch/hyper-v-virtual-switch
## Create and manage containers
### 🔳 create Windows Server container images
*Resources:* <br />
[Container Base Images | Microsoft Docs](https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/container-base-images)
### 🔳 manage Windows Server container images
*Resources:*

https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/container-base-images
### 🔳 configure Container networking
*Resources:*

https://docs.microsoft.com/en-us/virtualization/windowscontainers/container-networking/architecture
### 🔳 manage container instances
*Resources:*

https://docs.microsoft.com/en-us/virtualization/windowscontainers/wac-tooling/wac-containers
## Manage Azure Virtual Machines that run Windows Server
### ✅ manage data disks
![image](https://user-images.githubusercontent.com/51274282/152251828-b19c6a72-a70d-45f2-9fbb-0f0c4ba852c0.png)

You can click on the disk and get some extra details, including the ability to select a different size for the disk (you can only go up in size).

![image](https://user-images.githubusercontent.com/51274282/152254359-ddc5135a-09af-426c-a0db-3364830f009f.png)

You can remove the data disk using the portal or with powershell like this
```powershell
$vm = Get-AzVM -ResourceGroupName "lab" -Name "dc02"

Remove-AzVMDataDisk -VM $vm -Name "Data"

Update-AzVM -ResourceGroupName "lab" -VM $vm
```

*Resources:* <br />
[Attach a managed data disk to a Windows VM by using the Azure portal | Microsoft Docs](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/attach-managed-disk-portal) <br />
[Microsoft Azure Managed Disk LIVE Resize | John Savill YouTube](https://www.youtube.com/watch?v=VrtAcOH6S-w)
### ✅ resize Azure Virtual Machines
After you create a virtual machine (VM), you can scale the VM up or down by changing the VM size.
> Note: In some cases, you must deallocate the VM first. This can happen if the new size is not avalable on the hardware cluster that is currently hosting the VM.

> Note: if the VM uses Premium Storage, make sure that you choose an **s** version of the size to get Premium Storage support

![image](https://user-images.githubusercontent.com/51274282/152105109-2798fe4d-898e-4a8d-a2e3-f7050c3c980c.png)

You can also use the [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/) to resize a VM

to see all your VMs
```bash
az vm list --output table
```
```
Name      ResourceGroup    Location    Zones
--------  ---------------  ----------  -------
client01  LAB              centralus
dc01      LAB              centralus
dc02      LAB              centralus
dc03      LAB              centralus
```
Get the compatible sizes
```bash
az vm list-vm-resize-options --resource-group "lab" --name "client01" --output table
```
```
MaxDataDiskCount    MemoryInMb    Name                    NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
4                   8192          Standard_D2a_v4         2                1047552           51200
8                   16384         Standard_D4a_v4         4                1047552           102400
16                  32768         Standard_D8a_v4         8                1047552           204800
32                  65536         Standard_D16a_v4        16               1047552           409600
32                  131072        Standard_D32a_v4        32               1047552           819200
32                  196608        Standard_D48a_v4        48               1047552           1228800
...
```
Resize the VM
```bash
az vm resize --resource-group lab -name client01 -size Standard_B2s
```
And you're done! 🎉

Like noted above if the size you need isn't listed you'll have to deallocate the VM first
```bash
az vm deallocate --resource-group lab --name client01

az vm resize --resource-group lab -name client01 -size Standard_B2s

az vm start --resource-group lab --name client01
```

*Resources:* <br />
[Change the size of a virtual machine | Microsoft Docs](https://docs.microsoft.com/en-us/azure/virtual-machines/resize-vm)
### 🔳 configure continuous delivery for Azure Virtual Machines
*Resources:*

https://docs.microsoft.com/en-us/azure/architecture/solution-ideas/articles/cicd-for-azure-vms
### 🔳 configure connections to VMs
*Resources:*

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/connect-logon

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/winrm
### 🔳 manage Azure Virtual Machines network configuration
*Resources:*

[Virtual networks and virtual machines in Azure | Microsoft Docs](https://docs.microsoft.com/en-us/azure/virtual-network/network-overview)
# <a name="4"></a>Implement and manage an on-premises and hybrid networking infrastructure
## Implement on-premises and hybrid name resolution
### 🔳 integrate DNS with AD DS
*Resources:*

https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/active-directory-integrated-dns-zones
### 🔳 create and manage zones and records
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816891(v=ws.10)

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816819(v=ws.10)
### 🔳 configure DNS forwarding/conditional forwarding
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816830(v=ws.10)

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794735(v=ws.10)
### 🔳 integrate Windows Server DNS with Azure DNS private zones
*Resources:*

https://docs.microsoft.com/en-us/azure/dns/private-dns-scenarios
### 🔳 implement DNSSEC
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj200221(v=ws.11)?WT.mc_id=modinfra-39512-orthomas
## Manage IP addressing in on-premises and hybrid scenarios
### 📚 implement and manage IPAM
*Resources:* <br />
[IP Address Management (IPAM) | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/networking/technologies/ipam/ipam-top)
### ⏹ implement and configure DHCP server role (on-premises only)
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd183630(v=ws.10)
### 🔳 resolve IP address issues in hybrid enviroments
*Resources:*

https://support.microsoft.com/en-au/topic/fix-duplicate-ip-address-conflicts-on-a-dhcp-network-d68499da-69a3-da3b-4630-d17e502adf50
### ⏹ create and manage scopes
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd183624(v=ws.10)
### ⏹ create and manage IP reservations
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd183698(v=ws.10)
### 🔳 implement DHCP high avalability
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn338978(v=ws.11)
## Implement on-premises and hybrid network connectivity
### 🔳 implement and manage Remote Access role
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn636119(v=ws.11)
### 🔳 implement and manage Azure Network Adapter
*Resources:*

https://docs.microsoft.com/en-us/azure/architecture/hybrid/azure-network-adapter
### 🔳 implement and manage Azure Extended Network
*Resources:*

https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/azure/azure-extended-network
### 🔳 implement and manage Network Policy Server role
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831683(v=ws.11)
### 🔳 implement Web Application Proxy
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn584107(v=ws.11)
### 🔳 implement Azure Relay
*Resources:*

https://docs.microsoft.com/en-us/azure/azure-relay/relay-what-is-it
### 🔳 implement site-to-site virtual private network (VPN)
*Resources:*

https://docs.microsoft.com/en-us/azure/vpn-gateway/point-to-site-about
### 🔳 implement Azure Virtual WAN
*Resources:*

https://docs.microsoft.com/en-us/azure/virtual-wan/virtual-wan-about
### 🔳 implement Azure AD Application Proxy
*Resources:*

https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/what-is-application-proxy
# <a name="5"></a>Manage storage and file services
## Configure and manage Azure File Sync
### 🔳 create Azure File Sync service
*Resources:*

https://docs.microsoft.com/en-us/azure/storage/file-sync/file-sync-deployment-guide?WT.mc_id=modinfra-39512-orthomas&tabs=azure-portal%2Cproactive-portal
### 🔳 create sync groups
*Resources:*

https://docs.microsoft.com/en-us/azure/storage/file-sync/file-sync-modify-sync-topology
### 🔳 create cloud endpoints
*Resources:*

https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share?WT.mc_id=modinfra-39512-orthomas&tabs=azure-portal
### 🔳 register servers
*Resources:*

https://docs.microsoft.com/en-us/azure/storage/file-sync/file-sync-server-registration
### 🔳 create server endpoints
*Resources:*

https://docs.microsoft.com/en-us/azure/storage/file-sync/file-sync-server-endpoint-create?WT.mc_id=modinfra-39512-orthomas&tabs=azure-portal
### 🔳 configure cloud tiering
*Resources:*

https://docs.microsoft.com/en-us/azure/storage/file-sync/file-sync-how-to-manage-tiered-files
### 🔳 monitor File Sync
*Resources:*

https://docs.microsoft.com/en-us/azure/storage/file-sync/file-sync-monitoring
### 🔳 migrate DFS to Azure File Sync
*Resources:*

https://docs.microsoft.com/en-us/azure/storage/files/files-manage-namespaces?WT.mc_id=modinfra-39512-orthomas&tabs=azure-portal
## Configure and manage Windows Server file shares
### ⏹ configure Windows Server file share access
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc784499(v=ws.10)
### 📚 configure file screens
*Resources:* <br />
[File Screening Management | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/storage/fsrm/file-screening-management)
### 📚 configure File Server Resource Manger (FSRM) quotas
*Resources:* <br />
[Quota Management | Microsoft Docs](https://docs.microsoft.com/en-us/windows-server/storage/fsrm/quota-management)
### 🔳 configure BranchCache
*Resources:*

https://docs.microsoft.com/en-us/windows-server/networking/branchcache/branchcache
### ⏹ impliment and configure Distributed File System (DFS)
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/dfs-namespaces/dfs-overview

https://docs.microsoft.com/en-us/windows-server/storage/dfs-replication/dfsr-overview
## Configure Windows Server storage
### ⏹ configure disks and volumes
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/disk-management/manage-disks

https://docs.microsoft.com/en-us/windows-server/storage/disk-management/manage-basic-volumes
### 🔳 configure and manage Storage Spaces
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/overview
### 🔳 configure and manage Storage Replica
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/storage-replica/storage-replica-overview
### ⏹ configure Data Deduplication
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/data-deduplication/overview
### 🔳 configure SMB direct
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/file-server/smb-direct
### 🔳 configure Storage Quality of Service (QoS)
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/storage-qos/storage-qos-overview
### ⏹ configure file systems
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/refs/refs-overview

https://docs.microsoft.com/en-us/windows-server/storage/file-server/ntfs-overview
