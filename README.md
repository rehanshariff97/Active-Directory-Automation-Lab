# Active-Directory-Automation-Lab
 # Enterprise Identity & Access Management (IAM) Automation

## Project Overview
This repository features a production-grade identity orchestration solution designed to automate bulk user provisioning natively within Active Directory Domain Services (AD DS). The solution parses structured human resources data (`.csv`) to automatically generate corporate identities, standardize account naming conventions, and assign granular resource attributes.



## Capabilities Demonstrated
* **Native Active Directory Integration:** Developed using the `ActiveDirectory` PowerShell core module, utilizing native cmdlets (`New-ADUser`, `Get-ADUser`) to directly modify the enterprise domain controller database.
* **Bulk Lifecycle Automation:** Eliminates manual administrative overhead by processing multi-attribute employee rosters instantly.
* **Data Normalization & Sanitization:** Implements algorithmic string manipulation to dynamically construct standardized corporate `SAMAccountName` and `UserPrincipalName` criteria.
* **Defensive Exception Handling:** Incorporates robust state checking to identify pre-existing network directory records, ensuring execution continuity without duplicative identity conflicts.
* **Security Compliance Enforcement:** Dynamically provisions temporary strong passwords while forcing an explicit object flag parameter requiring a password reset at next interactive network login (`-ChangePasswordAtLogon $true`).
