# Active Directory Lab in Microsoft Azure

## Overview

This project demonstrates the deployment and administration of a Windows Active Directory environment in Microsoft Azure.

I built a domain controller using Windows Server 2025 and a Windows 11 client workstation. I configured Active Directory Domain Services (AD DS), DNS, organizational units, users, security groups, Group Policy, and domain authentication.

I also performed a DNS troubleshooting scenario by intentionally misconfiguring the client DNS server, diagnosing the resulting domain resolution failure, and restoring connectivity.

## Lab Architecture

- **DC01** — Windows Server 2025
  - Active Directory Domain Services
  - DNS Server
  - Domain: `lab.local`
  - Private IP: `172.16.0.4`

- **CLIENT01** — Windows 11 Pro
  - Domain-joined workstation
  - Member of `lab.local`
  - Uses DC01 for internal DNS

- **Microsoft Azure**
  - Virtual network connecting DC01 and CLIENT01

## Skills Demonstrated

- Active Directory Domain Services (AD DS)
- Windows Server 2025
- Windows 11
- Microsoft Azure
- User and Group Administration
- Organizational Units (OUs)
- Security Groups
- Domain Joining
- DNS Configuration
- Group Policy
- Domain Authentication
- `gpupdate` and `gpresult`
- `nslookup` and `ipconfig`
- DNS Troubleshooting

## Active Directory Configuration

Created organizational units to organize domain resources:

- IT
- HR
- Finance
- Sales
- Workstation

Created and managed domain user accounts and security groups, including an IT user account and the `IT-Staff` security group.

## Domain Workstation

Joined CLIENT01 to the `lab.local` domain and configured the workstation to use DC01 as its DNS server.

Domain authentication was verified from CLIENT01 using:

`whoami`

`hostname`

`echo %logonserver%`

These commands confirmed that the domain user authenticated to `LAB` on CLIENT01 through DC01.

## Group Policy

Created and linked the `Workstation Security Policy` GPO to the Workstation OU.

Forced Group Policy processing using:

`gpupdate /force`

Verified the policy using:

`gpresult /scope computer /r`

The results confirmed that `Workstation Security Policy` was successfully applied to CLIENT01.

## Security Group Verification

Added the IT user to the `IT-Staff` security group and verified group membership from CLIENT01 using:

`whoami /groups`

## DNS Troubleshooting Scenario

To simulate a common Active Directory connectivity problem, I intentionally changed CLIENT01's DNS server from the domain controller (`172.16.0.4`) to Google's public DNS server (`8.8.8.8`).

Running:

`nslookup lab.local`

resulted in:

`Non-existent domain`

This demonstrated that public DNS could not resolve the internal Active Directory domain.

I restored CLIENT01's DNS server to `172.16.0.4`, flushed the DNS cache using:

`ipconfig /flushdns`

and verified successful domain resolution with:

`nslookup lab.local`

This restored communication with the Active Directory DNS environment.

## Key Takeaways

This project provided hands-on experience deploying and administering a Windows domain environment, managing Active Directory objects, applying Group Policy, configuring client DNS, verifying domain authentication, and troubleshooting DNS-related domain connectivity issues.
## Lab Screenshots

### Active Directory Structure
Created and organized Active Directory objects including organizational units, users, and the CLIENT01 domain workstation.

![Active Directory Structure](AD%20File%20names%20and%20Organization.png)

### Active Directory Virtual Machines
Deployed Windows Server 2025 as the domain controller and Windows 11 as the client workstation in Microsoft Azure.

![Azure AD Lab](AD%20VM.png)

### Domain Authentication Verification
Verified that John Smith successfully authenticated to the LAB domain from CLIENT01 using `whoami`, `hostname`, and `%logonserver%`.

![Domain Authentication](whoami%20JSmith%20commands.png)

### Security Group Membership
Added John Smith to the IT-Staff security group and verified his domain group membership from CLIENT01.

![IT Staff Group](Group%20Name%20look%20up%20on%20Jsmith%20Client.png)

### Group Policy
Created and linked a workstation Group Policy Object and verified that CLIENT01 received the policy.

![Group Policy Verification](Group%20Policy%20Command%20Line.png)

### DNS Troubleshooting
Intentionally changed CLIENT01's DNS server to `8.8.8.8`, reproduced a failure to resolve the internal `lab.local` domain, then restored DNS to DC01 (`172.16.0.4`) and confirmed successful name resolution.

![DNS Troubleshooting](DNS%20Troubleshoot.png)

## What I Learned

This lab provided hands-on experience with deploying and administering a Windows Active Directory environment. I practiced domain administration, user and group management, Group Policy, domain authentication, DNS configuration, and troubleshooting connectivity between a domain controller and client workstation.
