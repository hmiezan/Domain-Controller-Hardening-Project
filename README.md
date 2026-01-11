# Domain-Controller-Hardening-Project
As part of building a secure, enterprise‑grade Active Directory environment, I implemented a full hardening baseline for my domain controllers. This project focused on strengthening authentication, securing communication channels, reducing attack surface, and enabling deep visibility for detection and response.

## Objectives
- Protect the identity plane (AD DS, Kerberos, LDAP, DNS)
- Reduce lateral movement opportunities
- Enforce strong authentication and encryption
- Harden SMB, SYSVOL, and DC‑specific services
- Improve auditability and detection capabilities
- Build a foundation for SOC‑style monitoring and incident response

## Architecture Context
This hardening was applied to a multi‑DC environment running on Proxmox:
- DC1 (Primary Domain Controller, PDC Emulator)
- DC2 (Secondary Domain Controller)
- VLAN‑segmented network with ASA firewall
- Windows Server 2022
- Hybrid‑ready design for future Azure integration

## Security Controls Implemented
1. Authentication Hardening: Strengthened identity authentication to prevent credential replay, downgrade attacks, and weak encryption.

Key Measures:

- Enforced NTLMv2 only
- Disabled LM and NTLMv1
- Required Kerberos AES‑based encryption
- Blocked legacy RC4 cipher usage
- Prevented storage of LM hashes

Impact:
Eliminates weak authentication paths and significantly reduces credential theft risk.

#2. LDAP Signing & Channel Binding : Protected LDAP traffic against man‑in‑the‑middle attacks.
Key Measures:
- Required LDAP server signing
- Required LDAP client signing
- Enforced channel binding tokens
Impact:
Ensures all LDAP communication is integrity‑checked and tamper‑resistant.

3. SMB & SYSVOL Hardening : Secured file replication and administrative shares.
Key Measures:
- Disabled SMBv1
- Enforced SMB signing (client + server)
- Restricted anonymous access
- Hardened SYSVOL permissions
Impact:
Prevents exploitation of legacy SMB protocols and strengthens SYSVOL integrity.

4. Service Minimization
Reduced attack surface by disabling unnecessary services.
Services Disabled:
- Remote Registry
- Fax
- SSDP Discovery
- UPnP Device Host
- Bluetooth Support
- Offline Files
- Error Reporting
Impact:
Minimizes lateral movement paths and reduces exploitable services.

5. Privilege & Logon Restrictions : Implemented strict access control on domain controllers.
Key Measures:
- Only Domain Admins allowed local logon
- Denied local logon for all non‑privileged accounts
- Restricted RDP access to privileged groups
- Enforced least privilege for service accounts
Impact:
Prevents unauthorized access and reduces insider threat exposure.

6. Windows Firewall Hardening : Locked down inbound traffic to only essential domain controller services.
Allowed:
- LDAP / LDAPS
- Kerberos
- DNS
- SMB
- DFSR
- RDP (restricted)
Impact:
Creates a zero‑trust perimeter around the DCs.

## Validation & Testing
After applying the hardening baseline, I validated:
- domain health
- LDAP signing enforcement
- SMB signing enforcement
- SYSVOL replication (DFSR)
- Event logs for auditing completeness
- Firewall rule effectiveness
All tests passed successfully.

## Outcome
This project resulted in a fully hardened, enterprise‑grade Active Directory environment with:
• 	Strong authentication
• 	Secure communication channels
• 	Reduced attack surface
• 	Enhanced monitoring and detection
• 	SOC‑ready audit trails
• 	A resilient identity foundation for hybrid cloud expansion
It demonstrates my ability to design, secure, and operate critical identity infrastructure following modern security best practices.
