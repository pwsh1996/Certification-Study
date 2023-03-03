| Skills Measured | Percent of Exam |
| --- | --- |
| [1.0 Security Architecture](#1) | 29% |
| [2.0 Security Operations](#2) | 30% |
| [3.0 Security Engineering and Cyptography](#3) | 26% |
| [4.0 Governance, Risk, and Compliance](#4) | 15% |

🔳[38] Needs to be Studied
📚[?] Read the Docs
⏹[?] Did at Work
✅[?] Studied and did Hands-On Testing

# <a name="1"></a>1.0 Security Architecture
## 1.1 Given a scenario, analyze the security requirements and objectives to ensure an appropriate, secure network architecture for a new or existing network.
### 🔳 Services
#### Load balancer
#### Intrusion detection system (IDS)/ network intrusion detection system (NIDS)/ wireless intrusion detection system (WIDS) 
#### Instrusion prevention system (IPS)/ network intrusion prevention system (NIPS)/ wireless intrusion prevention system (WIPS)
#### Web application firewall (WAF)
#### Network access control (NAC)
#### Virtual private network (VPN)
#### Domain Name System Security Extensions (DNSSEC)
#### Firewall/ unified threat management (UTM)/ next-generation firewall (NGFW) 
#### Network address translation (NAT) gateway
#### Internet gateway
#### Forward/transparent proxy
#### Reverse proxy
#### Distributed denial-of-service (DDOS) protection
#### Routers
#### Mail security
#### Application programming interface (API) gateway/ Extensible Markup Language (XML) gateway
#### Traffic mirroring
##### Switched port analyzer (SPAN) ports
##### Port mirroring
##### Virtual private cloud (VPC)
##### Network tap
#### Sensors
##### Security information and event management (SIEM)
##### File integrity monitoring (FIM)
##### Simple Network Management Protocol (SNMP) traps
##### NetFlow
##### Data loss prevention (DLP)
##### Antivirus
### 🔳 Segmentation
#### Microsegmentation
#### Local area network (LAN)/ virtual local area network (VLAN)
#### Jump box
#### Screened subnet
#### Data zones
#### Staging environments
#### Guest environments
#### VPC/ virtual network (VNET)
#### Availability zone
#### NAC lists
#### Policies / security groups
#### Regions
#### Access control lists (ACLs)
#### Peer-to-peer
#### Air gap
### 🔳 Deperimeterization/zero trust
#### Cloud
#### Remote work
#### Mobile
#### Outsourcing and contracting
#### Wireless/ radio frequency (RF) networks
### 🔳 Merging of networks from various organizations
#### Peering
#### Cloud to on premises
#### Data sensitivity levels
#### Mergers and acquisitions
#### Cross-domain
#### Federation
#### Directory services
### 🔳 Software-defined networking (SDN)
#### Open SDN
#### Hybrid SDN
#### SDN overlay

## 1.2 Given a scenario, analyze the organizational requirements to determine the proper infrastructure security design.
### 🔳 Scalability
#### Vertically
#### Horizontally
### 🔳 Resiliency
#### High availability
#### Diversity/ heterogeneity
#### Course of action orchestration
#### Distributed allocation
#### Redundancy
#### Replication
#### Clustering
### 🔳 Automation
#### Autoscaling
#### Security Orchestration, Automation, and Response (SOAR)
#### Bootstrapping
### 🔳 Performance
### 🔳 Containerization
### 🔳 Virtualization
### 🔳 Content delivery network
### 🔳 Caching

## 1.3 Given a scenario, integrate software applications securely into an enterprise architecture.
### 🔳 Baseline and templates
#### Secure design patterns/ types of web technologies
##### Storage design patterns
#### Container APIs
#### Secure coding standards
#### Application vetting processes
#### API management
#### Middleware
### 🔳 Software assurance
#### Sandboxing/ development environment
#### Validating thrid-party libraries
#### Defined DevOps pipeline
#### Code signing
#### Interactive application security testing (IAST) vs. dynamic application security testing (DAST) vs. static application security testing (SAST)
### 🔳 Considerations of integrating enterprise applications
#### Customer relationship management (CRM)
#### Enterprise resource planning (ERP)
#### Configuration management database (CMDB)
#### Content management system (CMS)
#### Integration enablers
##### Directory services
##### Domain name system (DNS)
##### Service-oriented architecture (SOA)
##### Enterprise service bus (ESB)
### 🔳 Integrating security into development life cycle
#### Formal methods
#### Requirements
#### Fielding
#### Insertions and upgrades
#### Disposal and reuse
#### Testing
##### Regression
##### Unit testing
##### Integration testing
#### Development approaches
##### SecDevOps
##### Agile
##### Waterfall
##### Spiral
##### Versioning
##### Continuous integration/ continuous delivery (CI/CD) pipelines
#### Best practices
##### Open Web Application Security Project (OWASP)
##### Proper Hypertext Transfer Protocol (HTTP) headers

## 1.4 Given a scenario, implement data security techniques for securing enterprise architecture.
### 🔳 Data loss prevention
#### Blocking use of external media
#### Print blocking
#### Remote Desktop Protocol (RDP) blocking
#### Clipboard privacy controls
#### Restricted virtual desktop infrasucture (VDI) implementation 
#### Data classification blocking
### 🔳 Data loss detection
#### Watermarking
#### Digital rights management (DRM)
#### Network traffic decryption/ deep packet inspection
#### Network traffic analysis
### 🔳 Data classification, labeling, and tagging
#### Metadata/attributes
### 🔳 Obfuscation
#### Tockenization
#### Scrubbing
#### Masking
### 🔳 Anonymization
### 🔳 Encrypted vs. unencrypted
### 🔳 Data life cycle
#### Create
#### Use
#### Share
#### Store
#### Archive
#### Destroy
### 🔳 Data inventory and mapping
### 🔳 Data integrity management
### 🔳 Data storage, backup, and recovery
#### Redundant array of inexpensive disks (RAID)

## 1.5 Given a scenario, analyze the security requirements and objectives to provide the appropriate authentication and authorization controls.
### 🔳 Credential management
#### Password repository application
##### End-user password storage
##### On premises vs. cloud repository
#### Hardware key manager
#### Privileged access management
### 🔳 Password policies
#### Complexity
#### Length
#### Character classes
#### History
#### Maximum/minimum age
#### Auditing
#### Reversable encryption
### 🔳 Federation
#### Transitive trust
#### OpenID
#### Security Assertion Markup Language (SAML)
#### Shibboleth
### 🔳 Access control
#### Mandatory access control (MAC)
#### Discretionary access control (DAC)
#### Role-based access control
#### Rule-based access control
#### Attribute-based access control
### 🔳 Protocols
#### Remote Authentication Dial-in User Server (RADIUS)
#### Terminal Access Controller Access Control System (TACACS)
#### Diameter
#### Lightweight Directory Access Protocol (LDAP)
#### Kerberos
#### OAuth
#### 802.1X
#### Extsible Authentication Protocol (EAP)
### 🔳 Multifactor authentication (MFA)
#### Two-factor authentication (2FA)
#### 2-Step verification
#### In-band
#### Out-of-band
### 🔳 One-time password (OTP)
#### HMAC-based one-time password (HOTP)
#### Time-based one-time password (TOTP)
### 🔳 Hardware root of trust
### 🔳 Single sing-on (SSO)
### 🔳 JavaScript Object Notation (JSON) web tocken (JWT)
### 🔳 Attestation and identity proofing

## 1.6 Given a set of requirements, implement secure cloud and virtualization solutions.
### 🔳 Virtualization strategies
#### Type 1 vs. Type 2 hypervisors
#### Containers
#### Emulation
#### Application virtualization
#### VDI
### 🔳 Provisioning and deprovisioning
### 🔳 Middleware
### 🔳 Metadata and tags
### 🔳 Deployment models and considerations
#### Business directives
##### Cost
##### Scalability
##### Resources
##### Location
##### Data protection
#### Cloud deployment models
##### Private
##### Public
##### Hybrid
##### Community
### 🔳 Hosting models
#### Multitenant
#### Single-tenant
### 🔳 Service models
#### Software as a service (SAAS)
#### Platform as a serice (PAAS)
#### Infrastucture as a service (IAAS)
### 🔳 Cloud provider limitations
#### Internet Protocol (IP) address scheme
#### VPC peering
### 🔳 Extending appropriate on-premises controls
### 🔳 Storage models
#### Object storage/file-based storage
#### Database storage
#### Block storage
#### Blob storage
#### Key-value pairs

## 1.7 Explain how cryptography and public key infrastructure (PKI) support security objectives and requirements.
### 🔳 Privacy and confidentiality requirements
### 🔳 Integrity requirements
### 🔳 Non-repudiation
### 🔳 Compliance and policy requirements
### 🔳 Common cryptography use cases
#### Data at rest
#### Data in transit
#### Data in process/data in use
#### protection of web services
#### Embedded systems
#### Key escrow/management
#### Mobile security
#### Secure authentication
#### Smart card
### 🔳 Common PKI use cases
#### Web services
#### Email
#### Code signing 
#### Federation
#### Trust models
#### VPN
#### Enterprise and security automation/orchestration

## 1.8 Explain the impact of emerging technologies on enterprise security and privacy.
### 🔳 Artificial intelligence
### 🔳 Machine learning
### 🔳 Quantum computing
### 🔳 Blockchain
### 🔳 Homomorphic encryption
#### Private information retrieval
#### Secure function evaluation
#### Private function evaluation
### 🔳 Secure multiparty computation
### 🔳 Distributed consensus
### 🔳 Big Data
### 🔳 Virtual/augmented reality
### 🔳 3-D printing
### 🔳 Passwordless authentication
### 🔳 Nano technology
### 🔳 Deep learning
#### Natual language processing
#### Deep fakes
### 🔳 Biometric impersonation

# <a name="2"></a>2.0 Security Operations
## 2.1 Given a scenario, perform threat management activities.
### 🔳 Intelligence types
#### Tactical
##### Commodity malware
#### Strategic
##### Targeted attacks
#### Operational
##### Threat hunting
##### Threat emulation
### 🔳 Actor types
#### Advanced persistent threat (APT)/nation-state
#### Insider threat
#### Competitor
#### Hacktivist
#### Script kiddie
#### Organized crime
### 🔳 Threat actor properties
#### Resource
##### Time
##### Money
#### Suppy chain access
#### Create vulnerabilities
#### Capabilities/sophistication
#### Identifying techniques
### 🔳 Intelligence collection methods
#### Intelligence feeds
#### Deep web
#### Proprietary
#### Open-source intelligence (OSINT)
#### Human intelligence (HUMINT)
### 🔳 Frameworks
#### MITRE Adversarial Tactics, Techniques, & Common knowledge (ATT&CK)
##### ATT&CK for industrial control systems (ICS)
#### Diamond Model of Intrusion Analysis
#### Cyber Kill Chain

## 2.2 Given a scenario, analyze indicators of compromise and formulatee ab appropriate response.
### 🔳 Indicators of compromise
#### Packet capture (PCAP)
#### Logs
##### Network logs
##### Vulnerability logs
##### Operating system logs
##### Access logs
##### NetFlow logs
#### Notifications
##### FIM alerts 
##### SIEN alerts
##### DLP alerts
##### IDS/IPS alerts
##### Antivirus alerts
#### Notification serverity/priorities
#### Unusual process activity
### 🔳 Response
#### Firewall rules
#### IPS/IDS rules
#### ACL rules
#### Signature rules
#### Behavior rules
#### DLP rules
#### Scripts/regular expressions

## 2.3 Given a scenario, perform vulnerability management activities.
### 🔳 Vulnerability scans
#### Credentialed vs. non-credentialed
#### Agent-based/server-based
#### Criticality ranking
#### Active vs. passive
### 🔳 Security Content Automation Protocol (SCAP)
#### Extensible Configuration Checklist Description Format (XCCDF)
#### Open Vulnerability and Assessment Language (OVAL)
#### Common Platform Enumeration (CPE)
#### Common Vulnerabilites and Exposures (CVE)
#### Common Vulnerability Scoring System (CVSS)
#### Common Configuration Enumeration (CCE)
#### Asset Reporting Format (ARF)
### 🔳 Self-assessment vs. third-party vendor assessment
### 🔳 Patch management
### 🔳 Information sources
#### Advisories
#### Bulletins
#### Vendor websites
#### Information Sharing and Analysis Centers (ISACs)
#### News reports

## 2.4 Given a scenario, use the appropriate vulnerability assessment and penetration testing methods and tools.
### 🔳 Methods
#### Static analysis
#### Dynamic analysis
#### Side-channel analysis
#### Reverse engineering
##### Software
##### Hardware
#### Wireless vulnerability scan
#### Software composition analysis
#### Fuzz testing
#### Pivoting
#### Post-exploitation
#### Persistence
### 🔳 Tools
#### SCAP scanner
#### Network traffic analyzer 
#### Vulnerability scanner 
#### Protocol analyzer
#### Port scanner
#### HTTP interceptor
#### Exploit framework
#### Password cracker
### 🔳 Dependency management
### 🔳 Requirements
#### Scope of work
#### Invasive vs. non-invasive
#### Asset inventory
#### Permissions and access
#### Corporate policy considerations
#### Facility considerations
#### Physical security considerations
#### Rescan for corrections/changes

## 2.5 Given a scenario, analyze vulnerabilities and recommend risk mitigations.
### 🔳 Vulnerabilities
#### Race conditions
#### Overflows
##### Buffer
##### Integer
#### Broken authentication
#### Unsecure references
#### Poor exception handling
#### Security misconfiguration
#### Improper headers
#### Information disclosure
#### Certificate errors
#### Weak cryptography implementations
#### Weak ciphers
#### Weak cipher suite implementations
#### Software composition analysis
#### Use of vulnerable frameworks and software modules
#### Use of unsafe functions
#### Third-party libraries
##### Dependencies
#### Code injections/malicious changes
#### End of support/end of life 
#### Regression issues
### 🔳 Inherently vulnerable system/application
#### Client-side processing vs. server-side processing
#### JSON/representaional state tranfer (REST)
#### Browser extensions
##### Flash
##### ActiveX
#### Hypertext Markup Language 5 (HTML5)
#### Asynchronous JavaScript and XLM (AJAX)
#### Simple Object Access Protocol (SOAP)
#### Machine code vs. bytecode or interpreted vs. emulated
### 🔳 Attacks
#### Directory traversal
#### Cross-site scripting (XSS)
#### Cross-site request forgery (CSRF)
#### Injection
##### XML
##### LDAP
##### Structured Query Language (SQL)
##### Command
##### Process
#### Sandbox escape
#### Virtual machine (VM) hopping
#### VM escape
#### Border Gateway Protocol (BGP)/ route hijacking 
#### Interception attacks
#### Denial-of-service (DOS)/DDOS
#### Authentication bypass
#### Social engineering
#### VLAN hopping

## 2.6 Given a scenario, use processes to reduce risk.
### 🔳 Proactive and detection
#### Hunts
#### Developing countermeasures
#### Deceptive technologies
##### Honeynet
##### Honeypot
##### Decoy files
##### Simulators
##### Synamic network configurations
### 🔳 Security data analytics
#### Processing pipelines
##### Data
##### Stream
#### Indexing and search
#### Log collection and curation
#### Database activity monitoring
### 🔳 Preventive
#### Antivirus
#### Immutable systems
#### Hardening
#### Sandbox detonation
### 🔳 Application control
#### License technologies
#### Allow list vs. block list 
#### Time of check vs. time of use
#### Atomic execution
### 🔳 Security automation
#### Cron/scheduled tasks
#### Bash
#### PowerShell
#### Python
### 🔳 Physical security
#### Review of lighting
#### Review of visitor logs
#### Camera reviews
#### Open spaces vs. confined spaces

## 2.7 Given an incident, implement the appropriate response.
### 🔳 Event classifications
#### Flase positive
#### False negative
#### True positive
#### True negative
### 🔳 Triage event
### 🔳 Preescalation tasks
### 🔳 Incident response process
#### Preperation
#### Detection
#### Analysis
#### Containment
#### Recovery
#### Lessons learned
### 🔳 Specific response playbooks/processes
#### Scenarios
##### Ransomeware
##### Data exfiltration
##### Social engineering
#### Non-automated response methods
#### Automated response methods
##### Runbooks
##### SOAR
### 🔳 Communication plan
### 🔳 Stakeholder management

## 2.8 Explain the importance of forensic concepts.
### 🔳 Legal vs. internal corporate purposes
### 🔳 Forensic process
#### Identification
Ensures the scene is safe, the evidence is not contaminated, and the scope of the evidence is properly identified
#### Evidence collection
Ensures authorization to collect the evidence

Legal - Probably a warrant

Internal Corporate - Supervisor's approval or other internal process
##### Chain of custody
The record of evidence history from tcollection to court presentation and disposal

Items should be placed in specialized evidence bags to ensure electronic media cannot be damaged or corrupted by electronic discharge (ESD)

if the device has wireless it should go into a Faraday Bag - (Shields devices from outside signals to prevent data from being altered, deleted, or added to a device)

Legal Hold - Preserves all relevant information when litigation is reasonably expected to occur
##### Order of volatility
Collect evidence that could be modified or destroyed the easiest first

[RFC 3227](https://www.rfc-editor.org/rfc/rfc3227.html)
###### Memory snapshots
###### Images
##### Cloning
#### Evidence preservation
##### Secure storage
##### Backups
#### Analysis
Creates a copy of the evidence for analysis

- Media Analysis <br />
Disk image - Creates a bit-by-bit copy of a hard disk or USB drive, including the slack space and unallocated space on the drive - Make a Cain of Custody and then a hash digest of the image

Memory snapshot/dump - Conducts memory capture and forensics in a very similar processes used in disk imaging - 
- Software Analysis <br />

- Network Analysis <br />

- Hardware or Embedded Device Analysis <br />

##### Forensics tools
#### Verification
#### Presentation
Creates a report of the methods and tools used an then presents detailed findings and conclusions based on the analysis performed
### 🔳 Integrity preservation
#### Hashing
### 🔳 Cryptanalysis
### 🔳 Steganalysis

## 2.9 Given a scenario, use forensic analysis tools.
### 🔳 File carving tools
#### Foremost
#### Strings
### 🔳 Binary analysis tools
#### Hex dump
#### Binwalk
#### Ghidra
#### GNU Project debugger (GDB)
#### OllyDbg
#### readelf
#### objdump
#### strace
#### ldd
#### file
### 🔳 Analysis tools
#### ExifTool
#### Nmap
#### Aircrack-ng
#### Volatility
#### The Sleuth Kit
#### Dynamically vs. statically linked
### 🔳 Imaging tools
#### Forensic Toolkit (FTK) Imager
#### dd
### 🔳 Hashing utilities
#### sha256sum
#### ssdeep
### 🔳 Live collection vs post-mortem tools
#### netstat
#### ps
#### vmstat
#### ldd
#### lsof
#### netcat
#### tcpdump
#### conntrack
#### Wireshark

# <a name="3"></a>3.0 Security Engineering and Cyptography
## 3.1 Given a scenario, apply secure configurations to enterprise mobility.
### 🔳 Managed configurations
#### Application control
#### Password
#### MFA requirements
#### Tocken-based access
#### Patch repository
#### Firmware Over-theAir
#### Remote wipe
#### WiFi
##### WiFi Protected Access (WPA2/3)
##### Device certificates
#### Profiles
#### Bluetooth
#### Near-field communication (NFC)
#### Peripherals
#### Geofencing
#### VPN settings
#### Geotagging
#### Certificate management
#### Full device encryption
#### Tethering 
#### Airplane mode
#### Location services
#### DNS over HTTPS (DoH)
#### Custom DNS
### 🔳 Deployment scenarios
#### Bring your own device (BYOD)
#### Corporate-owned
#### Corporate-owned, personally enabled (COPE) 
#### Choose your own device (CYOD)
### 🔳 Security considerations
#### Unauthorized remote activation/deactivation of devices or features
#### Encrypted and unencrypted communication concerns
#### Physical reconnaissance
#### Personal data theft
#### Health privacy
#### Implications of wearable devices
#### Digital forensics of collected data
#### Unauthorized application stores
#### Jailbreaking/rooting
#### Side loading
#### Containerization
#### Original equipment manufacturer (OEM) and carrier differences
#### Supply chain issues
#### eFuse

## 3.2 Given a scenario, configure and implement endpoint security controls.
### 🔳 Hardening techniques
#### Removing unneeded services
#### Disabling unused accounts
#### Images/templates
#### Remove end-of-life devices
#### Remove end-of-service devices
#### Local drive encryption
#### Enable no execute (NX)/execute never (XN) bit
#### Disable central processing unit (CPU) virtualization support
#### Secure encrypted enclaves/memory encyption
#### Shell restrictions
#### Address space layout randomization (ASLR)
### 🔳 Processes
#### Patching
##### Firmware
##### Application
#### Logging
#### Monitoring
### 🔳 Mandatory access control
#### Security-Enhanced Linux (SELinux)/Security-Enhanced Android (SEAndroid)
#### Kernel vs. middleware
### 🔳 Trustworthy computing
#### Trusted Platform Module (TPM)
#### Secure Boot
#### Unified Extensible Firmware Interface (UEFI)/basic imput/output system (BIOS) protection
#### Attestation services
#### Hardware security module (HSM)
#### Measured boot
#### Self-encrypting drives (SEDs)
### 🔳 Compensating controls
#### Antivirus
#### Application controls
#### Host-based intrusion detection system (HIDS)/Host-based intrusion prevention system (HIPS)
#### Host-based firewall
#### Endpoint detection and response (EDR)
#### Redundant hardware
#### Self-healing hardware
#### User and entity behavior analysis (UEBA)

## 3.3 Explain security considerations impacting specific sectors and operational technologies.
### 🔳 Embedded
#### Internet of Things (IoT)
#### System on a chip (SoC)
#### Application-specific integrated circuit (ASIC)
#### Field-programmable gate array (FPGA)
### 🔳 ICS/supervisory control and data acquisition (SCADA)
#### Programmable logic controller (PLC)
#### Historian
#### Ladder logic
#### Safty instrumented system
#### Heating, ventilation, and air conditioning (HVAC)
### 🔳 Protocols
#### Controller Area Network (CAN) bus
#### Modbus
#### Distributed Network Protocol 3 (DNP3)
#### Zigbee
#### Common Industrial Protocol (CIP)
#### Data Distribution service
### 🔳 Sectors
#### Energy
#### Manufacturing
#### Healthcare
#### Public utilities
#### Public services
#### Facility services

## 3.4 Explain how cloud technology adoption impacts organizational security.
### 🔳 Automation and orchestration
### 🔳 Encryption configuration
### 🔳 Logs
#### Availability
#### Collection
#### Monitoring
#### Configuration
#### Alerting
### 🔳 Monitoring configurations
### 🔳 Key ownership and location
### 🔳 Key life-cycle management
### 🔳 Backup and recovery methods
#### Cloud as business continuity and disaster recovery (BCDR)
#### Primary provider BCDR
#### Alternative provider BCDR
### 🔳 Infrastructure vs. serverless computing
### 🔳 Application virtualization
### 🔳 Software-defined networking
### 🔳 Misconfigurations
### 🔳 Collaboration tools
### 🔳 Storage configurations
#### Bit splitting
#### Data dispersion
### 🔳 Cloud access security broker (CASB)

## 3.5 Given a business requirement, implement the appropriate PKI solution.
### 🔳 PKI hierarchy
#### Certificate authority (CA)
#### Subordinate/intermediate CA
#### Registration authority (RA)
### 🔳 Certificate types
#### Wildcard certificate
#### Extended validation
#### Multidomain
#### General purpose
### 🔳 Certification usages/profiles/templates
#### Client authentication
#### Server authentication
#### Digital signatures
#### Code signing
### 🔳 Extensions
#### Common name (CN)
#### Subject alternate name (SAN)
### 🔳 Trusted providers
### 🔳 Trust model
### 🔳 Cross-certification
### 🔳 Configure profiles
### 🔳 Life-cycle management
### 🔳 Public and private keys
### 🔳 Digital signature
### 🔳 Certificate pinning
### 🔳 Certificate stapling
### 🔳 Certificate signing requests (CSRs)
### 🔳 Online Certificate Status Protocol (OCSP) vs. certificate revocation list (CRL)
### 🔳 HTTP Strict Transport Security (HSTS) 

## 3.6 Given a business requirement, implement the appropriate cryptographic protocols and algorithms.
### 🔳 Hashing
**Hashing** - Takes an arbitrary length string as its input and transforms it into a fixed-length string as its output

Is used in [File Integrity Monitoring (FIPS)](https://en.wikipedia.org/wiki/File_integrity_monitoring)

Weak algorithms are susceptible to vulnerabilities such as a *hash collision*, where the output of two different inputs is the same.

A `Hashquine` is "A file that shows it's own hash" https://github.com/corkami/collisions/tree/master/hashquines and https://www.rogdham.net/2017/03/12/gif-md5-hashquine.en

#### Secure Hashing Algorithm (SHA)
In 2015 NIST prohibits the use of the SHA-1 hashing function within US federal agencies

Windows Command Prompt:
```Bash
certutil -hashfile ZoomInstaller.exe sha256
```
[FIPS 180-1](https://csrc.nist.gov/publications/detail/fips/180/1/archive/1995-04-17) (SHA-1) https://en.wikipedia.org/wiki/SHA-1 <br />
[FIPS 180-2](https://csrc.nist.gov/publications/detail/fips/180/2/archive/2002-08-01) (SHA-2) <br />
[FIPS 180-4](https://csrc.nist.gov/publications/detail/fips/180/4/final) (Current) https://en.wikipedia.org/wiki/SHA-2 <br />
[FIPS 202](https://csrc.nist.gov/publications/detail/fips/202/final) (SHA-3) https://en.wikipedia.org/wiki/SHA-3<br />
#### Hash-based message authentication code (HMAC)
Combines a cryptographic hash of the message with a secret key (symmetric key)

*Reduces* collisions due to the addition of unique outputs

[RFC 2104](https://www.rfc-editor.org/rfc/rfc2104) <br />
[HMAC | Wikipedia](https://en.wikipedia.org/wiki/HMAC) <br />
#### Message digest (MD)

**Very Weak** https://www.mscs.dal.ca/~selinger/md5collision/ <br />

https://github.com/DavidBuchanan314/monomorph Monomorph is a tool that packs shell code into an exicutable and no matter what you put in it, it will always output the same hash.

A tool for detecting collisions like this can be found here https://github.com/cr-marcstevens/hashclash/tree/collisiondetection/src/collisiondetection. 

**Linux Bash:**
```bash
md5sum Autoruns.zip
```
Output:
```
c398f4249e7b677105ac754be08c24c1  Autoruns.zip
```
**Powershell:**
```powershell
get-filehash Autoruns.zip -Algorithm MD5
```
Output:
```
Algorithm       Hash                                              Path
---------       ----                                              ----
MD5             C398F4249E7B677105AC754BE08C24C1                  C:\Users\jacob.petrie\Downloads\Autoruns.zip
```
[RFC 1321](https://www.rfc-editor.org/rfc/rfc1321) <br />
[RFC 6151](https://www.rfc-editor.org/rfc/rfc6151) <br />
[MD5 | Wikipedia](https://en.wikipedia.org/wiki/MD5) <br />
#### RACE integrity primitives evaluation message digest (RIPEMD)
Powershell 5.1:
```powershell 5.1
Get-FileHash Autoruns.zip -Algorithm RIPEMD160
```
https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.ripemd160 <br />
[RIPEMD | Wikipedia](https://en.wikipedia.org/wiki/RIPEMD) <br />
#### Poly1305
[RFC 8439](https://www.rfc-editor.org/rfc/rfc8439.html) <br />
[Poly1305 | Wikipedia](https://en.wikipedia.org/wiki/Poly1305) <br />
### 🔳 Symmetric algorithms
#### Modes of operation
##### Galois/Counter Mode (GCM)
##### Electronic codebook (ECB)
##### Cipher block chaining (CBC)
##### Counter (CTR)
##### Output feedback (OFB)
#### Stream and block
##### Advanced Encryption Standard (AES)
##### Triple digital encryption standard (3DES)
##### ChaCha
##### Salsa20
### 🔳 Asymmetric algorithms
#### Key agreement
##### Diffie-Hellman
##### Elliptic-curve Diffie-Hellman (ECDH)
#### Signing
##### Digital signature algorithm (DSA) 
##### Rivest, Shamire, and Adleman (RSA)
##### Elliptic-curve digital signature algorithm (ECDSA)
### 🔳 Protocols
#### Secure Sockets Layer (SSL)/ Transport Layer Security (TLS)
#### Secure/Multipurpose Internet Mail Extensions (S/MIME)
#### Internet Protocol Security (IPSec)
#### Secure Shell (SSH)
#### EAP
### 🔳 Elliptic curve cryptography
#### P256
#### P384
### 🔳 Forward secrecy
### 🔳 Authenticated encryption with associated data
### 🔳 Key stretching
#### Password-based key derivation fuction 2 (PBKDF2)
#### Bcrypt

## 3.7 Given a scenario, troubleshoot issues with cryptographic implementations.
### 🔳 Implementation and configuration issues
#### Validity dates 
#### Wrong certificate type
#### Revoked certificates
#### Incorrect name
#### Chain issues
##### Invalid root or intermediate CAs
##### Self-signed
#### Weak signing algorithm
#### Weak cipher suite
#### Incorrect permissions
#### Cipher mismatches
#### Downgrade
### 🔳 Keys
#### Mismatched
#### Improper key handling
#### Embedded keys
#### Rekeying
#### Exposed private keys
#### Crypto shredding
#### Cryptographic obfuscation
#### Key rotation
#### Compromised keys

# <a name="4"></a>4.0 Governance, Risk, and Compliance
## 4.1 Given a set of requirements, apply the appropriate risk strategies.
### 🔳 Risk assessment
#### Likelihood
#### Impact
#### Qualitative vs. quantitative 
#### Exposure factor
#### Asset value
#### Total cost of ownership (TCO)
#### Return on investment (ROI)
#### Mean time to recovery (MTTR)
#### Mean time between failure (MTBF)
#### Anualized loss expectancy (ALE)
#### Annualized rate of occurrence (ARO)
#### Single loss expectancy (SLE)
#### Gap analysis
### 🔳 Risk handling techniques
#### Transfer
#### Accept
#### Avoid
#### Mitigate
### 🔳 Risk types
#### Inherent
#### Residual
#### Exceptions
### 🔳 Risk management life cycle
#### Identify
#### Assess
#### Control
##### People
##### Process
##### Technology
##### Protect
##### Detect
##### Respond
##### Restore
#### Review
#### Frameworks
### 🔳 Risk tracking
#### Risk register
#### Key performance indeicators
##### Scalability
##### Reliability
##### Availability
#### Key Risk indicators
### 🔳 Risk appetite vs. risk tolerance
#### Tradeoff analysis
#### Usability vs. security requirements
### 🔳 Policies and security practices
#### Separation of duties
#### Job rotation
#### Mandatory vacation
#### Least privilege
#### Employment and termination procedures
#### Training and awareness for users
#### Auditing requirements and frequency

## 4.2 Explain the importance of managing and mitigating vendor risk.
### 🔳 Shared responsibility model (role/responsibilities)
#### Cloud service provider (CSP)
##### Geopraphic location
##### Infrastructure
##### Compute
##### Storage
##### Networking
##### Services
#### Client
##### Encryption
##### Operating systems
##### Applications
##### Data
### 🔳 Vendor lock-in and vendor lockout
### 🔳 Vendor viability
#### Financial risk
#### Merger or acqusition risk
### 🔳 Meeting client requirements
#### Legal
#### Change management
#### Staff turnover
#### Device and technical configurations 
### 🔳 Support availability
### 🔳 Geographical considerations
### 🔳 Supply chain visibility
### 🔳 Incident reporting requirements
### 🔳 Source code escrows
### 🔳 Ongoing vender assessment tools
### 🔳 Thrid-party dependencies
#### Code
#### Hardware
#### Modules
### 🔳 Technical considerations
#### Technical testing
#### Network segmentation
#### Transmission control
#### Shared credentials

## 4.3 Explain compliance frameworks and legal considerations, and their organizational impact.
### 🔳 Security concerns of integrating diverse industries
### 🔳 Data considerations
#### Data sovereignty
#### Data ownership
#### Data classifications
#### Data retention
#### Data types
##### Health
##### Financial
##### Intellectual property
##### Personally identifiable information (PII)
#### Data removal, destruction, and sanitization
### 🔳 Geographic considerations
#### Location of data
#### Location of data subject
#### Location of cloud provider
### 🔳 Third-party attestation of compliance
### 🔳 Regulations, accreditations, and standards
#### Payment Card Industry Data Security Standard (PCI DSS)
#### General Data Protection Regulation (GDPR)
#### Internation Organization for Standardization (ISO)
#### Capability Maturity Model Integration (CMMI)
#### National Institure of Standards and Technology (NIST)
#### Children's Online Privacy Protection Act (COPPA)
#### Common Criteria
#### Cloud Security Alliance (CSA) Security Trust Assurance and Risk (STAR)
### 🔳 Legal considerations
#### Due diligence
#### Due care
#### Export controls
#### Legal holds
#### E-discovery
### 🔳 Contract and agreement types
#### Service-level agreement (SLA)
#### Master service agreement (MSA)
#### Non-disclosure agreement (NDA)
#### Memorandum of understanding (MOU)
#### Interconnection security agreement (ISA)
#### Operational-level agreement
#### Privacy-level agreement

## 4.4 Explain the importance of business continuity and disaster recovery concepts.
### 🔳 Business impact analysis
#### Recovery point objective
#### Recovery time objective
#### Recovery service level
#### Mission essential functions
### 🔳 Privacy impact assessment
### 🔳 Disaster recovery plan (DRP)/ business continuity plan (BCP)
#### Cold site
#### Warm site
#### Hot site
#### Mobile site
### 🔳 Incident response plan
#### Roles/responsibilities
#### After-action reports
### 🔳 Testing plans
#### Checklist
#### Walk-through
#### Tabletop exercises
#### Full interruption test
#### Parallel test/simulation test
