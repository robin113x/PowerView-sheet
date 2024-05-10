# PowerView-sheet

---

# PowerShell Commands for RED TEAM OPERATION

## Get Current Domain

### Get all items

- **Script**: [PowerView.ps1](https://github.com/PowerShellMafia/PowerSploit/blob/dev/Recon/PowerView.ps1)

#### Basic Info
| Command           | Description              |
|-------------------|--------------------------|
| `Get-Domain`      | **Get info about the current domain** |
| `Get-DomainUser`  | **Get info about the current domain** |


---

## PowerShell Commands

| Category        | Command                                             | Description                         |
|-----------------|-----------------------------------------------------|-------------------------------------|
| **Policy**      | `Get-DomainPolicy`                                  | Get info about the policy.          |
|                 | `(Get-DomainPolicy)."KerberosPolicy"`               | Kerberos tickets info (MaxServiceAge) |
|                 | `(Get-DomainPolicy)."SystemAccess"`                 | Password policy                     |
|                 | `(Get-DomainPolicy).PrivilegeRights`                | Check your privileges               |
|                 | `Get-DomainP\| Format-ListolicyData`                              | Same as Get-DomainPolicy            |
| **Computers**   | `Get-NetComputer \| Select-Object samaccountname, operatingsystem` | Get information about computers in the domain |
|                 | `Get-NetComputer -Unconstrained`                    | Get information about computers in the domain without constraints |
|                 | `Get-NetComputer -TrustedToAuth \| Select-Object samaccountname` | Find computers with Constrained Delegation |
|                 | `Get-DomainGroup -AdminCount`                       | Get information about domain groups with AdminCount attribute set |
| **Domain Controller** | `Get-DomainController`                             | Get information about domain controllers |
|                 | `Get-DomainController \| Select-Object Forest, Domain, IPAddress, Name, OSVersion \| Format-List` | Get specific info of the current domain controller |

---


# PowerShell Commands for User Operations
 
## Users

| PowerShell Command                                       | Description                              |
|----------------------------------------------------------|------------------------------------------|
| `Get-DomainUser -Properties name, MemberOf ` | Get usernames and their groups           |
| `Get-NetUser`                                            | Get-DomainUser and Get-NetUser are kind of the same |
| `Get-NetUser \| Select-Object samaccountname, description, pwdlastset, logoncount, badpwdcount` | Get detailed information about users |
| `Get-DomainUser -LDAPFilter "Description=* *" \| select name, description`                       | Get all Description of  user   |
| `Get-NetUser -UserName student107`                       | Get information about a specific user   |
| `Get-NetUser -Properties name, description`              | Get all user descriptions                |
| `Get-NetUser -Properties name, pwdlastset, logoncount, badpwdcount` | Get various user attributes               |
| `Find-UserField -SearchField Description -SearchTerm "built"` | Search for user accounts by description containing "built" |


# Computers

| PowerShell Command                                                        | Description                                                                        |
|-----------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `Get-DomainComputer -Properties DnsHostName`                                | Get all domain names of computers                                                 |
| `Get-NetComputer`                                                           | Get all computer objects                                                          |
| `Get-NetComputer -Ping`                                                     | Send a ping to check if the computers are working                                 |
| `Get-NetComputer -Unconstrained`                                            | Domain Controllers (DCs) always appear but aren't useful for privilege escalation |
| `Get-NetComputer -TrustedToAuth`                                            | Find computers with Constrained Delegation                                         |
| `Get-DomainGroup -AdminCount \| Get-DomainGroupMember -Recurse \| ?{$_.MemberName -like '*$'}` | Find any machine accounts in privileged groups                                    |




# Groups 

| PowerShell Command                                                        | Description                                                                        |
|-----------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `Get-DomainGroup \| where Name -like "*Admin*" \| select SamAccountName`   | Get all groups with names containing "Admin"                                        |
| `Get-DomainGroupMember -Identity "Domain Admins" -Recurse    \|select GroupDomain,GroupName,MemberName`      | Get all admin groups                                                                    |
| `Get-DomainGroup *admin* \|select name`                                                             | Get all admin groups                                                                    |
| `Get-NetGroup`                                                             | Get all groups                                                                    |
| `Get-NetGroup -Domain mydomain.local`                                      | Get groups of a specific domain                                                    |
| `Get-NetGroup 'Domain Admins'`                                              | Get all data of the "Domain Admins" group                                           |
| `Get-NetGroup -AdminCount \| select name,memberof,admincount,member \| Format-List` | Search for admin groups                                                        |
| `Get-NetGroup -UserName "myusername"`                                       | Get groups of a user                                                               |
| `Get-NetGroupMember -Identity "Administrators" -Recurse`                   | Get users inside the "Administrators" group, including users in nested groups       |
| `Get-NetGroupMember -Identity "Enterprise Admins" -Domain mydomain.local`   | Get users inside the "Enterprise Admins" group (exists only in the forest root domain) |
| `Get-NetLocalGroup -ComputerName dc.mydomain.local -ListGroups`            | Get local groups of a machine (requires admin rights)                               |
| `Get-NetLocalGroupMember -computername dcorp-dc.dollarcorp.moneycorp.local`| Get users of local groups on a computer                                             |
| `Get-DomainObjectAcl -SearchBase 'CN=AdminSDHolder,CN=System,DC=testlab,DC=local' -ResolveGUIDs` | Check AdminSDHolder users                                                |
| `Get-DomainObjectACL -ResolveGUIDs -Identity * \| ? {$_.SecurityIdentifier -eq $sid}` | Get ObjectACLs by SID                                                    |
| `Get-NetGPOGroup`                                                          | Get restricted groups                                                              |

# Computers

| PowerShell Command                                                        | Description                                                                        |
|-----------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `Get-DomainComputer -Properties DnsHostName`                                | Get all domain names of computers                                                 |
| `Get-NetComputer`                                                           | Get all computer objects                                                          |
| `Get-NetComputer -Ping`                                                     | Send a ping to check if the computers are working                                 |
| `Get-NetComputer -Unconstrained`                                            | Domain Controllers (DCs) always appear but aren't useful for privilege escalation |
| `Get-NetComputer -TrustedToAuth`                                            | Find computers with Constrained Delegation                                         |
| `Get-DomainGroup -AdminCount \| Get-DomainGroupMember -Recurse \| ?{$_.MemberName -like '*$'}` | Find any machine accounts in privileged groups                                    |

# Organization Units (OUs)

| PowerShell Command                                                        | Description                                                                        |
|-----------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `Get-DomainOU -Properties Name \| sort -Property Name`                      | Get names of OUs                                                                   |
| `Get-DomainOU "Servers" \| %{Get-DomainComputer -SearchBase $_.distinguishedname -Properties Name}` | Get all computers inside an OU (e.g., "Servers")                              |
| `Get-NetOU`                                                                 | Get Organization Units                                                             |
| `Get-NetOU StudentMachines \| %{Get-NetComputer -ADSPath $_}`               | Get all computers inside an OU (e.g., "StudentMachines")                           |






# Logon and Sessions

| PowerShell Command                                                        | Description                                                                        |
|-----------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| `Get-NetLoggedon -ComputerName <servername\HYDRA.local>`                               | Get currently logged on users on a specific computer (requires admin rights)        |
| `Get-NetSession`                                                           | Get active sessions on the local host                                              |
| `Get-LoggedOnLocal -ComputerName <servername>`                              | Get currently logged on users on a specific computer (requires remote registry access, default in server OS) |
| `Get-LastLoggedon -ComputerName <servername>`                               | Get the last user logged on to a specific computer (requires admin rights)          |
| `Get-NetRDPSession -ComputerName <servername>`                              | List RDP sessions on a specific computer (requires admin rights)                    |











![alt text](https://media.licdn.com/dms/image/D4D2DAQE6LZ7NUHFDSw/profile-treasury-image-shrink_800_800/0/1715372767689?e=1715979600&v=beta&t=-3N5j8XMnoJ8b0_8vUMMWLlrsMZU1hTIuzoWmRmY6yo)
