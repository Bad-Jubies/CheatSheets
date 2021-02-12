
# AD Enum
---

## ADSI Searcher

Reading:
  - [ 'adsiSearcher' Type Accelerator to Search Active Directory](https://devblogs.microsoft.com/scripting/use-the-powershell-adsisearcher-type-accelerator-to-search-active-directory/)

```powershell
# Get user's or machine's SID
$SID =  ([System.Security.Principal.NTAccount] "$domain\$username").Translate([System.Security.Principal.SecurityIdentifier]).Value
# Find all properties for the user with an ADSI Searcher
$searcher = [adsisearcher]""
$searcher.SearchRoot = "LDAP://$domain/<SID=$SID>"
$adInfo = $searcher.FindAll().properties
```
