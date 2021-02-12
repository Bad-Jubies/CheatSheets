
# AD Enum
---

## ADSI Searcher

# Get user's SID
$SID =  ([System.Security.Principal.NTAccount] "$domain\$username").Translate([System.Security.Principal.SecurityIdentifier]).Value
# Find all properties for the user with an ADSI Searcher
$searcher = [adsisearcher]"(&(objectClass=user)(objectCategory=person
$searcher.SearchRoot = "LDAP://$domain/<SID=$SID>"
$adInfo = $searcher.FindAll().properties
