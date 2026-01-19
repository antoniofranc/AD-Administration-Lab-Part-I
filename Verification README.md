# ✅ Verification

## Task 1 Verification
Verify New Users Created
```
# Check if users exist
Get-ADUser -Filter {Name -like "*Cepheus*" -or Name -like "*Starchaser*" -or Name -like "*Callisto*"} | Select-Object Name, SamAccountName, EmailAddress, Enabled
```

Verify Users Removed
```
# Try to find removed users (should return nothing)
Get-ADUser -Filter {Name -eq "Mike O'Hare" -or SamAccountName -eq "pvalencia"}
```

Verify Adam Masters Unlocked
```
Get-ADUser -Identity amasters -Properties LockedOut, PasswordExpired, ChangePasswordAtLogon | Select-Object Name, LockedOut, PasswordExpired, ChangePasswordAtLogon
```

## Task 2 Verification
Verify OU Created
```
Get-ADOrganizationalUnit -Filter {Name -eq "Security Analysts"} | Select-Object Name, DistinguishedName
```

Verify Security Group Created
```
Get-ADGroup -Filter {Name -eq "Security Analysts"} | Select-Object Name, GroupScope, GroupCategory
```

Verify Group Members
```
Get-ADGroupMember -Identity "Security Analysts" | Select-Object Name, SamAccountName
```
Verify Users Moved to Correct OU
```
Get-ADUser -Filter {SamAccountName -like "acepheus" -or SamAccountName -like "ostarchaser" -or SamAccountName -like "acallisto"} | Select-Object Name, DistinguishedName
```

## Task 3 Verification
Verify GPO Created and Linked
```
# Check if GPO exists
Get-GPO -Name "Security Analysts Control"

# Check GPO links
Get-GPInheritance -Target "OU=Security Analysts,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL"
```
Verify GPO Settings (GUI)

1. Open Group Policy Management
2. Navigate to Security Analysts Control GPO
3. Click Settings tab to view all configured policies

Generate GPO Report
```
# Generate HTML report
Get-GPOReport -Name "Security Analysts Control" -ReportType HTML -Path "C:\GPO-Report.html"

# Open the report
Start-Process "C:\GPO-Report.html"
```
## Task 4 Verification 
✅ Computer Successfully Joined to Domain
```
# From Domain Controller
Get-ADComputer -Identity "ACADEMY-IAD-W10" | Select-Object Name, DNSHostName, Enabled
```
✅ Computer in Correct OU
```
# Verify OU location
Get-ADComputer -Identity "ACADEMY-IAD-W10" -Properties CanonicalName | Select-Object Name, CanonicalName
```
```
Expected output:
Name              : ACADEMY-IAD-W10
CanonicalName     : INLANEFREIGHT.LOCAL/Corp/Employees/HQ-NYC/IT/Security Analysts/ACADEMY-IAD-W10
```

✅ Group Policy Applied
```
# From the Windows 10 client
gpresult /r /scope:computer
```
Look for Security Analysts Control in the applied GPOs list.

✅ Computer Can Authenticate Domain Users

1. Restart the Windows 10 client
2. At login screen, verify you can log in with domain credentials
3. Try logging in as one of the Security Analysts:
- Username: INLANEFREIGHT\acepheus
- Password: (the password they set after first login)

## Common Issues - Task 4

Cannot Join Domain - DNS Issues

Error: "The specified domain either does not exist or could not be contacted"

Solution:
```
# Check DNS settings
Get-DnsClientServerAddress

# Set DNS to point to Domain Controller
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses "10.129.202.146"

# Test DNS resolution
nslookup INLANEFREIGHT.LOCAL
```
Computer Already Exists in AD

Error: "The account already exists"

Solution:

```
# From Domain Controller, remove old computer object
Remove-ADComputer -Identity "ACADEMY-IAD-W10" -Confirm:$false

# Then try joining again from the client
```
Access Denied When Moving Computer
Error: "Access is denied"
Solution:

- Ensure you're using the htb-student_adm account
- Verify the account has Domain Admin privileges
- Check that you're running PowerShell/ADUC as Administrator



Group Policy Not Applying

Issue: After moving computer, Group Policy not taking effect

Solution:
```
# Force GPO update
gpupdate /force

# Restart computer
Restart-Computer -Force

# Check replication status
repadmin /replsummary
```















