# Deploy and Manage Active Directory Domain Services (AD DS) in on-premises and cloud environments *(30-35%)*
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

*Resources:* 

https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/install-active-directory-domain-services--level-100-

https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/install-a-new-windows-server-2012-active-directory-forest--level-200-

https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/what-s-new-in-active-directory-domain-services-installation-and-removal

https://docs.microsoft.com/en-us/powershell/module/addsdeployment/install-addsforest?view=windowsserver2022-ps
### 🔳 deploy and manage domain controllers in Azure
*Resources:*

https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/identity/adds-extend-domain
### 🔳 deploy Read-Only Domain Controllers (RODCs)
*Resources:*

https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/rodc/install-a-windows-server-2012-active-directory-read-only-domain-controller--rodc---level-200-
### 🔳 troubleshoot flexible single master operations (FSMO) roles
*Resources:*

https://docs.microsoft.com/en-us/troubleshoot/windows-server/identity/fsmo-roles
## Configure and manage multi-site, multi-domain, and multi-forest environments
### 🔳 configure and manage forest and domain trusts
*Resources:*

https://docs.microsoft.com/en-us/azure/active-directory-domain-services/tutorial-create-forest-trust

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771568(v=ws.10)
### 🔳 configure and manage AD DS sites
*Resources:*

https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/plan/understanding-active-directory-site-topology
### 🔳 configure and manage AD DS replication
*Resources:*

https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/replication/active-directory-replication-concepts
## Create and manage AD DS security principals
### 🔳 create and manage AD DS users and groups
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc754661(v=ws.10)

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771069(v=ws.10)
### 🔳 manage users and groups in multi-domain and multi-forest scenarios
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc733001(v=ws.10)
### 🔳 implement group managed service accounts (gMSAs)
*Resources:*

https://docs.microsoft.com/en-us/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview
### 🔳 join Windows Server to AD DS, Azure AD DS, and Azure AD
*Resources:*

https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/deployment/join-a-computer-to-a-domain

https://docs.microsoft.com/en-us/azure/active-directory-domain-services/join-windows-vm
## Impliment and manage hybrid identities
### 🔳 implement Azure AD Connect
*Resources:*

https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-azure-ad-connect-v2
### 🔳 manage Azure AD Connect Synchrinization
*Resources:*

https://docs.microsoft.com/en-us/azure/active-directory/hybrid/concept-azure-ad-connect-sync-architecture
### 🔳 implement Azure AD Connect cloud sync
*Resources:*

https://docs.microsoft.com/en-us/azure/active-directory/cloud-sync/what-is-cloud-sync
### 🔳 integrate Azure AD, AD DS, and Azure AD DS
*Resources:*

https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/identity/azure-ad
### 🔳 manage Azure AD DS
*Resources:*

https://docs.microsoft.com/en-us/azure/active-directory-domain-services/tutorial-create-instance
### 🔳 manage Azure AD Connect Health
*Resources:*

https://docs.microsoft.com/en-us/azure/active-directory/hybrid/whatis-azure-ad-connect
### 🔳 manage authentication in on-premises and hybrid enviroments
*Resources:*

https://docs.microsoft.com/en-us/azure/active-directory/hybrid/choose-ad-authn
### 🔳 configure and manage AD DS passwords
*Resources:*

https://docs.microsoft.com/en-us/azure/active-directory-domain-services/password-policy
## Manage Windows Server by using  domain-based Group Policies
### 🔳 implement Group Policy in AD DS
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831791(v=ws.11)
### 🔳 implement Group Policy Preferences in AD DS
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/desktop/policy/group-policy-preferences
### 🔳 implement Group Policy in Azure AD DS
*Resources:*

https://docs.microsoft.com/en-us/azure/active-directory-domain-services/manage-group-policy
# Manage Windows Servers and workloads in a hybrid environment *(10-15%)*
## Manage Windows Servers in a hybrid environment
### 🔳 deploy a Windows Admin Center gateway server
*Resources:*

https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/azure/deploy-wac-in-azure
### 🔳 configure a target machine for Windows Admin Center
*Resources:*

https://docs.microsoft.com/en-us/windows-server/manage/windows-admin-center/azure/manage-vm
### 🔳 configure PowerShell Remoting
*Resources:*

https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/running-remote-commands?view=powershell-7.2&viewFallbackFrom=powershell-7.1%3FWT.mc_id%3Dmodinfra-39512-orthomas

https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/powershell-remoting-faq?view=powershell-7.2&viewFallbackFrom=powershell-7.1%3FWT.mc_id%3Dmodinfra-39512-orthomas
### 🔳 configure CredSSP or Kerberos delegation for second hop remoting
*Resources:*

https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/ps-remoting-second-hop?view=powershell-7.2&viewFallbackFrom=powershell-7.1%3FWT.mc_id%3Dmodinfra-39512-orthomas
### 🔳 configure JEA for Powershell Remoting
*Resources:*

https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/jea/overview?view=powershell-7.2&viewFallbackFrom=powershell-7.1%3FWT.mc_id%3Dmodinfra-39512-orthomas
## Manage Windows Servers and workloads by using Azure services
### 🔳 manage Windows Servers by using Azure Arc
*Resources:*

https://docs.microsoft.com/en-us/azure/azure-arc/servers/onboard-windows-admin-center
### 🔳 assign Azure Policy Guest Configure
*Resources:*

https://docs.microsoft.com/en-us/azure/azure-arc/servers/learn/tutorial-assign-policy-portal?WT.mc_id=modinfra-39512-orthomas
### 🔳 deploy Azure services using Azure Virtual Machine on non-Azure machines
*Resources:*

https://docs.microsoft.com/en-us/azure/azure-arc/servers/learn/tutorial-enable-vm-insights?WT.mc_id=modinfra-39512-orthomas
### 🔳 manage updates for Windows machines
*Resources:*

https://docs.microsoft.com/en-us/azure/automation/update-management/overview?WT.mc_id=modinfra-39512-orthomas
### 🔳 integrate Windows Servers with Log Analytics
*Resources:*

https://docs.microsoft.com/en-us/azure/azure-monitor/agents/log-analytics-agent#network-requirements?WT.mc_id=modinfra-39512-orthomas
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
# Manage virtual machines and containers *(15-20%)*
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

*Resources:*

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/learn-more/use-local-resources-on-hyper-v-virtual-machine-with-vmconnect

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/learn-more/hyper-v-virtual-machine-connect
### 🔳 manage VM using PowerShell Remoting, PowerShell Direct, and HVC.exe
*Resources:*

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/manage-windows-virtual-machines-with-powershell-direct

https://techcommunity.microsoft.com/t5/itops-talk-blog/manage-hyper-v-vms-using-powershell-direct/bc-p/1531743/highlight/true
### 🔳 configure nested virtualization
*Resources:*

https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/user-guide/nested-virtualization
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

*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831766(v=ws.11)

https://docs.microsoft.com/en-us/powershell/module/hyper-v/get-vmmemory
### 🔳 configure Integrated Services
*Resources:*

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services
### 🔳 configure Discrete Device Assignment
*Resources:*

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/deploy/deploying-graphics-devices-using-dda

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/deploy/deploying-storage-devices-using-dda
### 🔳 configure VM Resource Groups
*Resources:*

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/manage-hyper-v-minroot-2016
### 🔳 configure VM CPU Groups
*Resources:*

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/manage-hyper-v-cpugroups
### 🔳 configure hypervisor scheduling types
*Resources:*

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/about-hyper-v-scheduler-type-selection
### 🔳 manage VM Checkpoints
*Resources:*

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/about-hyper-v-scheduler-type-selection

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/enable-or-disable-checkpoints-in-hyper-v
### 🔳 implement high availability for virtual machines
*Resources:*

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn743844(v=ws.11)
### 🔳 manage VHD and VHDX files
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831446(v=ws.11)
### 🔳 configure Hyper-V network adapter
*Resources:*

https://docs.microsoft.com/en-us/azure-stack/hci/concepts/host-network-requirements
### 🔳 configure NIC teaming
*Resources:*

https://docs.microsoft.com/en-us/azure-stack/hci/concepts/host-network-requirements

https://docs.microsoft.com/en-us/windows-server/networking/technologies/nic-teaming/nic-teaming
### 🔳 configure Hyper-V switch
*Resources:*

https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v-virtual-switch/hyper-v-virtual-switch
## Create and manage containers
### 🔳 create Windows Server container images
*Resources:*

https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/container-base-images
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
### 🔳 manage data disks
*Resources:*

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/attach-managed-disk-portal
### 🔳 resize Azure Virtual Machines
*Resources:*

https://docs.microsoft.com/en-us/azure/virtual-machines/resize-vm?WT.mc_id=modinfra-39512-orthomas&tabs=portal
### 🔳 configure continuous delivery for Azure Virtual Machines
*Resources:*

https://docs.microsoft.com/en-us/azure/architecture/solution-ideas/articles/cicd-for-azure-vms
### 🔳 configure connections to VMs
*Resources:*

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/connect-logon?WT.mc_id=modinfra-39512-orthomas

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/winrm?WT.mc_id=modinfra-39512-orthomas
### 🔳 manage Azure Virtual Machines network configuration
*Resources:*

https://docs.microsoft.com/en-us/azure/virtual-network/network-overview?WT.mc_id=modinfra-39512-orthomas
# Implement and manage an on-premises and hybrid networking infrastructure *(15-20%)*
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
### 🔳 implement and manage IPAM
*Resources:*

https://docs.microsoft.com/en-us/windows-server/networking/technologies/ipam/ipam-top
### 🔳 implement and configure DHCP server role (on-premises only)
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd183630(v=ws.10)
### 🔳 resolve IP address issues in hybrid enviroments
*Resources:*

https://support.microsoft.com/en-au/topic/fix-duplicate-ip-address-conflicts-on-a-dhcp-network-d68499da-69a3-da3b-4630-d17e502adf50
### 🔳 create and manage scopes
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd183624(v=ws.10)
### 🔳 create and manage IP reservations
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd183698(v=ws.10)?WT.mc_id=modinfra-39512-orthomas
### 🔳 implement DHCP high avalability
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn338978(v=ws.11)?WT.mc_id=modinfra-39512-orthomas
## Implement on-premises and hybrid network connectivity
### 🔳 implement and manage Remote Access role
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn636119(v=ws.11)?WT.mc_id=modinfra-39512-orthomas
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

https://docs.microsoft.com/en-us/azure/vpn-gateway/point-to-site-about?WT.mc_id=modinfra-39512-orthomas
### 🔳 implement Azure Virtual WAN
*Resources:*

https://docs.microsoft.com/en-us/azure/virtual-wan/virtual-wan-about?WT.mc_id=modinfra-39512-orthomas
### 🔳 implement Azure AD Application Proxy
*Resources:*

https://docs.microsoft.com/en-us/azure/active-directory/app-proxy/what-is-application-proxy
# Manage storage and file services *(15-20%)*
## Configure and manage Azure File Sync
### 🔳 create Azure File Sync service
*Resources:*

https://docs.microsoft.com/en-us/azure/storage/file-sync/file-sync-deployment-guide?WT.mc_id=modinfra-39512-orthomas&tabs=azure-portal%2Cproactive-portal
### 🔳 create sync groups
*Resources:*

https://docs.microsoft.com/en-us/azure/storage/file-sync/file-sync-modify-sync-topology?WT.mc_id=modinfra-39512-orthomas
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
### 🔳 configure Windows Server file share access
*Resources:*

https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc784499(v=ws.10)
### 🔳 configure file screens
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/fsrm/file-screening-management
### 🔳 configure File Server Resource Manger (FSRM) quotas
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/fsrm/quota-management
### 🔳 configure BranchCache
*Resources:*

https://docs.microsoft.com/en-us/windows-server/networking/branchcache/branchcache
### 🔳 impliment and configure Distributed File System (DFS)
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/dfs-namespaces/dfs-overview

https://docs.microsoft.com/en-us/windows-server/storage/dfs-replication/dfsr-overview
## Configure Windows Server storage
### 🔳 configure disks and volumes
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/disk-management/manage-disks

https://docs.microsoft.com/en-us/windows-server/storage/disk-management/manage-basic-volumes
### 🔳 configure and manage Storage Spaces
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/overview
### 🔳 configure and manage Storage Replica
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/storage-replica/storage-replica-overview
### 🔳 configure Data Deduplication
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/data-deduplication/overview
### 🔳 configure SMB direct
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/file-server/smb-direct
### 🔳 configure Storage Quality of Service (QoS)
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/storage-qos/storage-qos-overview
### 🔳 configure file systems
*Resources:*

https://docs.microsoft.com/en-us/windows-server/storage/refs/refs-overview

https://docs.microsoft.com/en-us/windows-server/storage/file-server/ntfs-overview
