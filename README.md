# Active Directory Administration Part I

## 📋 Table of Contents
- Tasks
- Task 1: Manage Users
- Task 2: Manage Groups and OUs
- Task 3: Manage Group Policy Objects
---
## 🎯 Overview
This project simulates real-world Active Directory administration tasks that IT professionals perform daily. You'll work as a domain administrator for Inlanefreight corporation, handling helpdesk tickets and administrative tasks.

## Learning Objectives
- Add and remove user accounts in Active Directory
- Create and manage Organizational Units (OUs)
- Create and configure Security Groups
- Unlock user accounts and reset passwords
- Duplicate and modify Group Policy Objects (GPOs)
- Apply GPOs to specific OUs
<img width="1163" height="542" alt="helping-out" src="https://github.com/user-attachments/assets/278ca9a1-11d0-4140-97eb-e6cf6dea3651" />

----
## 📝 Tasks
## Task 1: Manage Users

**Objective**

Add three new hires, remove two inactive users, and unlock a locked account.

**Part A: Add New Hires**

Create the following users in the `Corp > Employees > HQ-NYC > IT` OU:
```
Full Name          Username      Email                             Display Name
Andromeda Cepheus  acepheus      a.cepheus@inlanefreight.local     Andromeda Cepheus
Orion Starchaser   ostarchaser   o.starchaser@inlanefreight.local  Orion Starchaser
Artemis Callisto   acallisto     a.callisto@inlanefreight.local    Artemis Callisto
```

**User Requirements**

- ✅ Password: `NewP@ssw0rd123!`
- ✅ User must change password at next logon
- ✅ Account enabled

-----
## 🔧 Method 1: PowerShell 
**Step 1: Import Active Directory Module**

`Import-Module -Name ActiveDirectory`

**Step 2: Create Users**

User 1: Andromeda Cepheus
```
New-ADUser -Name "Andromeda Cepheus" `
  -GivenName "Andromeda" `
  -Surname "Cepheus" `
  -SamAccountName "acepheus" `
  -UserPrincipalName "acepheus@inlanefreight.local" `
  -Path "OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL" `
  -AccountPassword (ConvertTo-SecureString "NewP@ssw0rd123!" -AsPlainText -Force) `
  -Enabled $true `
  -ChangePasswordAtLogon $true `
  -EmailAddress "a.cepheus@inlanefreight.local" `
  -DisplayName "Andromeda Cepheus"
```
User 2: Orion Starchaser
```
New-ADUser -Name "Orion Starchaser" `
  -GivenName "Orion" `
  -Surname "Starchaser" `
  -SamAccountName "ostarchaser" `
  -UserPrincipalName "ostarchaser@inlanefreight.local" `
  -Path "OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL" `
  -AccountPassword (ConvertTo-SecureString "NewP@ssw0rd123!" -AsPlainText -Force) `
  -Enabled $true `
  -ChangePasswordAtLogon $true `
  -EmailAddress "o.starchaser@inlanefreight.local" `
  -DisplayName "Orion Starchaser"
```
User 3: Artemis Callisto
```
New-ADUser -Name "Artemis Callisto" `
  -GivenName "Artemis" `
  -Surname "Callisto" `
  -SamAccountName "acallisto" `
  -UserPrincipalName "acallisto@inlanefreight.local" `
  -Path "OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL" `
  -AccountPassword (ConvertTo-SecureString "NewP@ssw0rd123!" -AsPlainText -Force) `
  -Enabled $true `
  -ChangePasswordAtLogon $true `
  -EmailAddress "a.callisto@inlanefreight.local" `
  -DisplayName "Artemis Callisto"
```

**🖱️ Method 2: GUI (Active Directory Users and Computers)**
**Step 1: Open ADUC**

1. Open Server Manager
2. Click **Tools → Active Directory Users and Computers**

**Step 2: Navigate to IT OU** 

Expand: `inlanefreight.local → Corp → Employees → HQ-NYC → IT`

**Step 3: Create New User**

Right-click IT → New → User

2. Fill in user details

<img width="400" height="772" alt="Screenshot 2026-01-16 172526" src="https://github.com/user-attachments/assets/4dfdcf16-4bc0-4440-ab26-2e6f281c3342" />

3. Set password and options

**Step 4: Add Email Address**
1. Right-click the user → Properties
2. Go to General tab
3. Enter email address
4. Click **OK**

<img width="400" height="762" alt="Screenshot 2026-01-16 172746" src="https://github.com/user-attachments/assets/fd0c5aa4-55f6-4a86-95a1-9dc47fee024b" />
<img width="400" height="608" alt="Screenshot 2026-01-16 172804" src="https://github.com/user-attachments/assets/4463b503-4b31-4525-9b3b-1f1d3f5f42c1" />

----
**Part B: Remove Inactive Users**
Remove these users found during audit:
- Mike O'Hare
- Paul Valencia

**🔧 PowerShell Method**
```
# Remove Mike O'Hare
Remove-ADUser -Identity "Mike O'Hare" -Confirm:$false

# Remove Paul Valencia
Remove-ADUser -Identity pvalencia -Confirm:$false
```
**🖱️ GUI Method**
1. Right-click Employees OU → Find
2. Type: `Mike O'Hare`
3. Click Find Now
4. Right-click user → **Delete**
5. Confirm deletion
<img width="400" height="737" alt="Screenshot 2026-01-16 195539" src="https://github.com/user-attachments/assets/fa1a2052-b18d-46b5-9eb7-37efa5c533d5" />

--------
**Part C: Unlock Adam Masters' Account**

<img width="400" height="654" alt="image" src="https://github.com/user-attachments/assets/3008e634-9fc3-4ef3-a118-539036b26a8a" />

Trouble Ticket:
- User: Adam Masters
- Issue: Account locked (incorrect password attempts)
- Priority: Medium
- Action Required: Unlock account, reset password, force password change

**🔧 PowerShell Method** 
```
# Unlock the account
Unlock-ADAccount -Identity amasters

# Reset password
Set-ADAccountPassword -Identity amasters -Reset `
  -NewPassword (ConvertTo-SecureString "NewP@ssw0rdReset!" -AsPlainText -Force)

# Force password change at next logon
Set-ADUser -Identity amasters -ChangePasswordAtLogon $true
```
**🖱️ GUI Method**
1. Find Adam Masters in ADUC
2. Right-click → Reset Password
3. Enter new password
Check both boxes:
- ☑️ User must change password at next logon
- ☑️ Unlock the user's account
<img width="400" height="756" alt="Screenshot 2026-01-16 203342" src="https://github.com/user-attachments/assets/de4cc7ce-008e-4f6b-8caa-b9ce788ab30e" />

## Task 2: Manage Groups and Organizational Units
**Objective**

Create a new OU called "Security Analysts" under the IT OU, create a corresponding security group, and add the new hires to this group.

**Requirements**
- ✅ OU Name: `Security Analysts`
- ✅ Location: `Corp > Employees > HQ-NYC > IT > Security Analysts`
- ✅ Security Group Name: `Security Analysts`
- ✅ Group Scope: Global
- ✅ Group Type: Security
- ✅ Members: Andromeda Cepheus, Orion Starchaser, Artemis Callisto

-----
**Part A: Create Organizational Unit**

`New-ADOrganizationalUnit -Name "Security Analysts" 
  -Path "OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL"`

**🖱️ GUI Method**
1. Navigate to **IT** OU
2. Right-click **IT → New → Organizational Unit**
3. Name: `Security Analysts`
4. Leave **Protect container from accidental deletion** checked

<img width="400" height="570" alt="Screenshot 2026-01-16 210904" src="https://github.com/user-attachments/assets/9eb12192-b6db-4a72-8e4e-34c4a15ae7a7" />

**Part B: Create Security Group**
🔧 PowerShell Method
```
New-ADGroup -Name "Security Analysts" `
  -SamAccountName "analysts" `
  -GroupCategory Security `
  -GroupScope Global `
  -DisplayName "Security Analysts" `
  -Path "OU=Security Analysts,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL" `
  -Description "Members of this group are Security Analysts under the IT OU"
```
**🖱️ GUI Method**
1- Right-click Security Analysts OU → New → Group
2- Configure group:
- Group name: Security Analysts
- Group scope: Global
- Group type: Security

<img width="400" height="473" alt="Screenshot 2026-01-16 214650" src="https://github.com/user-attachments/assets/a52c3197-bca4-4365-8dc1-908ab5228460" />





