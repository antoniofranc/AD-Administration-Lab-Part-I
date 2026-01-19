# Active Directory Administration Part I

## 📋 Table of Contents
- Task 1: Manage Users
- Task 2: Manage Groups and OUs
- Task 3: Manage Group Policy Objects
---
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

**Part C: Add Users to Security Group**
🔧 PowerShell Method
```
Add-ADGroupMember -Identity analysts -Members acepheus,ostarchaser,acallisto

```
Verify Membership
```
Get-ADGroupMember -Identity analysts
```

**🖱️ GUI Method**

1. Find user (e.g., Andromeda Cepheus)
2. Right-click → **Add to a group**
3. Type: Security Analysts
4. Click Check Names

<img width="500" height="850" alt="Screenshot 2026-01-16 215051" src="https://github.com/user-attachments/assets/4b728d14-4d73-4866-9b9a-746789358b4a" />


**Part D: Move Users to Security Analysts OU**

🔧 PowerShell Method

```
# Move Andromeda Cepheus
Move-ADObject -Identity "CN=Andromeda Cepheus,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL" `
  -TargetPath "OU=Security Analysts,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL"

# Move Orion Starchaser
Move-ADObject -Identity "CN=Orion Starchaser,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL" `
  -TargetPath "OU=Security Analysts,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL"

# Move Artemis Callisto
Move-ADObject -Identity "CN=Artemis Callisto,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL" `
  -TargetPath "OU=Security Analysts,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL"
```

**🖱️ GUI Method**

1. Drag and drop each user from IT OU to Security Analysts OU

2. Verify all three users are in the correct location

## Task 3: Manage Group Policy Objects

**Objective**

Duplicate the "Logon Banner" GPO, rename it to "Security Analysts Control", modify its settings, and apply it to the Security Analysts OU.

**GPO Requirements**

**User Configuration**
- ✅ Allow access to Command Prompt
- ✅ Allow access to PowerShell
- ✅ Block all removable storage access

**Computer Configuration**
- ✅ Maintain Logon Banner settings (from original GPO)
- ✅ Strengthen password policy:
- Minimum password length: 12 characters
- Password complexity: Enabled
- Maximum password age: 60 days
- Minimum password age: 1 day
- Password history: 24 passwords

**Part A: Duplicate and Link GPO**

🔧 PowerShell Method
```
# Copy the GPO
Copy-GPO -SourceName "Logon Banner" -TargetName "Security Analysts Control"

# Link to Security Analysts OU
New-GPLink -Name "Security Analysts Control" `
  -Target "OU=Security Analysts,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL" `
  -LinkEnabled Yes
```
<img width="500" height="279" alt="Screenshot 2026-01-16 215901" src="https://github.com/user-attachments/assets/056b8cc8-e9f1-4a0b-86a9-972306f2b04a" />

**🖱️ GUI Method** 

Step 1: Open Group Policy Management

`Server Manager → Tools → Group Policy Management`

Step 2: Copy GPO

Expand: **Forest → Domains → inlanefreight.local → Group Policy Objects**
Right-click Logon Banner → Copy

3. Right-click Group Policy Objects → Paste

4. In the dialog:
- Select Use the default permissions for new GPOs

5. Name the GPO: `Security Analysts Control`

Step 3: Link GPO to OU

1. Navigate to: `inlanefreight.local → Corp → Employees → HQ-NYC → IT → Security Analysts`

2. Right-click Security Analysts → `Link an Existing GPO`

3. Select Security Analysts Control


**Part B: Modify User Configuration Settings**

Step 1: Open GPO for Editing

Right-click Security Analysts Control → Edit

Step 2: Allow Command Prompt

1. Navigate to:

**User Configuration → Policies → Administrative Templates → System**

2. Find: **Prevent access to the command prompt**

3. Double-click to open

4. Select **Disabled** (this allows access)

**Step 3: Allow PowerShell**

1. In the same System folder
2. Find: Turn on Script Execution (if present)
3. Set to Enabled with Allow all scripts

**Step 4: Block Removable Storage**

Navigate to:
`User Configuration → Policies → Administrative Templates → System → Removable Storage Access`

2. Enable these policies (set each to Enabled):
- All Removable Storage classes: Deny all access

OR individually enable:
- CD and DVD: Deny read access
- CD and DVD: Deny write access
- Floppy Drives: Deny read access
- Floppy Drives: Deny write access
- Removable Disks: Deny read access
- Removable Disks: Deny write access
- Tape Drives: Deny read access
- Tape Drives: Deny write access
- WPD Devices: Deny read access
- WPD Devices: Deny write access

**Part C: Modify Computer Configuration Settings**

Step 1: Verify Logon Banner

1. Navigate to:

**Computer Configuration → Policies → Windows Settings → Security Settings → Local Policies → Security Options**

2. Verify these are configured (should already be set from the copied GPO):
- Interactive logon: Message text for users attempting to log on
- Interactive logon: Message title for users attempting to log on

Step 2: Configure Password Policy

Navigate to:
`Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy`

2. Configure the following settings:
```
Policy Setting                                     Value
Enforce password history                           24 passwords remembered
Maximum password age                               60 days
Minimum password age                               1 day
Minimum password length                            12 characters
Password must meet complexity requirements         Enabled
Store passwords using reversible encryption        Disabled
```

## Task 3 Add and Remove Computers To The Domain
**Objective**

Add the newly provisioned computer `ACADEMY-IAD-W10` to the INLANEFREIGHT domain and move it to the correct OU so that Group Policy can take effect properly.
```
Parameter                        Value
Target Computer IP
Computer Name                    ACADEMY-IAD-W10
Local Username                   image
Local Password
Domain Admin                     _adm
Domain Admin Password            ********
Target Domain                    INLANEFREIGHT.LOCAL
Target OU                        Security Analysts
```
**Part A: Join Computer to Domain**

This method is used when you're logged into the computer you want to join to the domain.

Step 1: Open PowerShell as Administrator

Step 2: Join the Domain
```
Add-Computer -DomainName INLANEFREIGHT.LOCAL -Credential INLANEFREIGHT\htb-student_adm -Restart
```
When prompted, enter the password: ***

Important: The -Restart parameter will automatically restart the computer after joining. The domain join will not take effect until the restart is complete.


**🔧 Method 2: PowerShell (Remote Computer)**

This method is used when you want to join a remote computer from the domain controller or another domain-joined machine.
```
Add-Computer -ComputerName ACADEMY-IAD-W10 `
  -LocalCredential ACADEMY-IAD-W10\image `
  -DomainName INLANEFREIGHT.LOCAL `
  -Credential INLANEFREIGHT\htb-student_adm `
  -Restart
```

**🖱️ Method 3: GUI**

Step 1: Open System Properties

Open Control Panel

Navigate to System and Security → System

<img width="500" height="634" alt="image" src="https://github.com/user-attachments/assets/f48cfa11-7528-4dee-a3f4-9fdc5ae8eba7" />

3. Alternatively, right-click This PC → Properties

Step 2: Access Computer Name Settings

1. In the System window, look for the Computer name section

2. Click Change settings

<img width="400" height="468" alt="image" src="https://github.com/user-attachments/assets/32d53e30-9bb2-46cc-b1f1-c5711ced5d24" />
<img width="400" height="470" alt="image" src="https://github.com/user-attachments/assets/237f48e6-744a-4708-bfa2-dffc965793ac" />

Step 3: Open Computer Name/Domain Changes

1. In the System Properties window, click the Change button

2. This is located next to "To rename this computer or change its domain or workgroup, click Change"

Step 4: Configure Domain Settings

1. Verify Computer Name: Check that it shows `ACADEMY-IAD-W10`

2. Select the Domain radio button
  
3. Enter: `INLANEFREIGHT.LOCAL`

Step 5: Provide Domain Credentials

1. A credential prompt will appear
2. Username: `_adm or INLANEFREIGHT\htb-student_adm`
3. Password: ****

Note: You may see a warning about NetBIOS name resolution. This is outside the scope of this lab - click through it.

Step 6: Welcome to the Domain

If successful, you'll see a welcome message:

"Welcome to the INLANEFREIGHT.LOCAL domain"

Step 7: Restart Required

<img width="400" height="409" alt="image" src="https://github.com/user-attachments/assets/1da5d03e-a1d9-4c68-a533-79a8cb2cd5e7" />
<img width="400" height="435" alt="image" src="https://github.com/user-attachments/assets/1ef80444-439d-4cf8-a311-30ad75ca2b7d" />
<img width="400" height="596" alt="image" src="https://github.com/user-attachments/assets/b3122e42-f312-4621-93e4-2cb6c9aa5edd" />
<img width="400" height="337" alt="image" src="https://github.com/user-attachments/assets/3e69d615-6f69-4cc2-a747-4f5b6cc1cdf9" />

**Part B: Move Computer to Correct OU**

After joining the domain, the computer will be placed in the default Computers container. We need to move it to the Security Analysts OU so that the proper Group Policy applies.

🔧 Method 1: PowerShell

Step 1: Verify Current Location

From the Domain Controller (ACADEMY-IAD-DC01), open PowerShell and check where the computer is located:
```
Get-ADComputer -Identity "ACADEMY-IAD-W10" -Properties * | Select-Object CN, CanonicalName, IPv4Address
```

The CanonicalName property shows the full path. You should see something like:
```
INLANEFREIGHT.LOCAL/Computers/ACADEMY-IAD-W10
```

Step 2: Move Computer to Security Analysts OU
```
Move-ADObject -Identity "CN=ACADEMY-IAD-W10,CN=Computers,DC=INLANEFREIGHT,DC=LOCAL" `
  -TargetPath "OU=Security Analysts,OU=IT,OU=HQ-NYC,OU=Employees,OU=Corp,DC=INLANEFREIGHT,DC=LOCAL"
```
Step 3: Verify New Location
```
Get-ADComputer -Identity "ACADEMY-IAD-W10" -Properties CanonicalName | Select-Object Name, CanonicalName
```
You should now see:
```
INLANEFREIGHT.LOCAL/Corp/Employees/HQ-NYC/IT/Security Analysts/ACADEMY-IAD-W10
```
🖱️ Method 2: GUI (Active Directory Users and Computers)
Step 1: Open ADUC on Domain Controller

From ACADEMY-IAD-DC01, open Server Manager
Click Tools → Active Directory Users and Computers

Step 2: Locate the Computer

Expand inlanefreight.local
Click on Computers container
Find ACADEMY-IAD-W10

Step 3: Move the Computer

Right-click ACADEMY-IAD-W10

Select Move

Step 4: Select Target OU

1. In the Move dialog, navigate to:

2. Corp → Employees → HQ-NYC → IT → Security Analysts


Select Security Analysts OU

Click OK

Step 5: Verify Computer is Moved

1. Navigate to: Corp → Employees → HQ-NYC → IT → Security Analysts

2. Verify ACADEMY-IAD-W10 is now in this OU


**Part C: Force Group Policy Update on Client**

After moving the computer to the correct OU, force a Group Policy update to apply the Security Analysts Control GPO immediately.

From the Windows 10 Client (ACADEMY-IAD-W10)
```
# Force immediate Group Policy update
gpupdate /force
```

Verify Applied Policies
```
powershell# Check which GPOs are applied
gpresult /r
```







