
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

## User Account Control Translation

```powershell
function Get-UACTranslation {
  Param ([int]$UAC)

  $PropertyFlags = @(
    "SCRIPT", "ACCOUNTDISABLE", "RESERVED", "HOMEDIR_REQUIRED", "LOCKOUT", "PASSWD_NOTREQD", "PASSWD_CANT_CHANGE", "ENCRYPTED_TEXT_PWD_ALLOWED", "TEMP_DUPLICATE_ACCOUNT", "NORMAL_ACCOUNT", "RESERVED", "INTERDOMAIN_TRUST_ACCOUNT", "WORKSTATION_TRUST_ACCOUNT", "SERVER_TRUST_ACCOUNT", "RESERVED", "RESERVED", "DONT_EXPIRE_PASSWORD", "MNS_LOGON_ACCOUNT", "SMARTCARD_REQUIRED", "TRUSTED_FOR_DELEGATION", "NOT_DELEGATED", "USE_DES_KEY_ONLY", "DONT_REQ_PREAUTH", "PASSWORD_EXPIRED", "TRUSTED_TO_AUTH_FOR_DELEGATION", "RESERVED", "PARTIAL_SECRETS_ACCOUNT", "RESERVED", "RESERVED", "RESERVED", "RESERVED", "RESERVED"
  )

  $Attributes = ""
  1..($PropertyFlags.Length) | Where-Object { $UAC -bAnd [math]::Pow(2, $_) } | ForEach-Object {
    If ($Attributes.Length -EQ 0) { $Attributes = $PropertyFlags[$_] }
    Else { $Attributes = $Attributes + " | " + $PropertyFlags[$_] }
  }
  Return $Attributes
}
```
