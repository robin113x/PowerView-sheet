# PowerView-sheet

---

# PowerShell Commands for Current Domain Operations

## Get Current Domain

### Get all items

- **Script**: [PowerView.ps1](https://github.com/PowerShellMafia/PowerSploit/blob/dev/Recon/PowerView.ps1)

#### Basic Info
| Command           | Description              |
|-------------------|--------------------------|
| `Get-Domain`      | **Required**. Your API key. |
| `Get-DomainUser`  | **Required**. Your API key. |


---

## PowerShell Commands

| Category        | Command                                             | Description                         |
|-----------------|-----------------------------------------------------|-------------------------------------|
| **Policy**      | `Get-DomainPolicy`                                  | Get info about the policy.          |
|                 | `(Get-DomainPolicy)."KerberosPolicy"`               | Kerberos tickets info (MaxServiceAge) |
|                 | `(Get-DomainPolicy)."SystemAccess"`                 | Password policy                     |
|                 | `(Get-DomainPolicy).PrivilegeRights`                | Check your privileges               |
|                 | `Get-DomainPolicyData`                              | Same as Get-DomainPolicy            |
| **Computers**   | `Get-NetComputer \| Select-Object samaccountname, operatingsystem` | Get information about computers in the domain |
|                 | `Get-NetComputer -Unconstrained`                    | Get information about computers in the domain without constraints |
|                 | `Get-NetComputer -TrustedToAuth \| Select-Object samaccountname` | Find computers with Constrained Delegation |
|                 | `Get-DomainGroup -AdminCount`                       | Get information about domain groups with AdminCount attribute set |
| **Domain Controller** | `Get-DomainController`                             | Get information about domain controllers |
|                 | `Get-DomainController \| Select-Object Forest, Domain, IPAddress, Name, OSVersion \| Format-List` | Get specific info of the current domain controller |

---
