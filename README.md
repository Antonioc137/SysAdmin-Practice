# SysAdmin-Practice# ⚙️ PowerShell Lab: Bulk Active Directory User Creation

> **Goal:** Use PowerShell to automate the creation of multiple user accounts in Active Directory from a CSV file.

---

## 🧩 Scenario

The IT department is onboarding 20 new hires. Manually creating AD user accounts for each is time-consuming and error-prone. I created a PowerShell script that reads a CSV file and bulk-creates user accounts with default settings.

---

## 📁 Sample CSV Format

```csv
FirstName,LastName,Username,Password
John,Doe,jdoe,P@ssword123
Jane,Smith,jsmith,P@ssword123
```

---

## 💻 Script Overview

```powershell
Import-Module ActiveDirectory
$Users = Import-Csv -Path "C:\NewUsers.csv"

foreach ($User in $Users) {
    $FullName = "$($User.FirstName) $($User.LastName)"
    $Username = $User.Username
    $Password = (ConvertTo-SecureString $User.Password -AsPlainText -Force)

    New-ADUser -Name $FullName `
               -GivenName $User.FirstName `
               -Surname $User.LastName `
               -SamAccountName $Username `
               -UserPrincipalName "$Username@domain.local" `
               -AccountPassword $Password `
               -Enabled $true `
               -Path "OU=NewHires,DC=domain,DC=local"
}
```

---

## ✅ Outcome

- Created 20 users in under 30 seconds.
- Password policies applied automatically.
- Script reused for future onboarding sessions.

---

## 🧰 Skills Demonstrated

- PowerShell scripting
- Active Directory administration
- Automation with CSV input
- Secure password handling
# 🐧 Bash Lab: Disk Usage Alert Script (Linux)

> **Goal:** Monitor disk usage on a Linux server and send an alert if usage exceeds 80%.

---

## 🧩 Scenario

A Linux-based server started experiencing performance issues due to full disk space. I created a Bash script to check `/` disk usage and send an alert if usage exceeded a defined threshold.

---

## 💻 Script Overview

```bash
#!/bin/bash

THRESHOLD=80
USAGE=$(df / | grep / | awk '{ print $5 }' | sed 's/%//g')

if [ $USAGE -gt $THRESHOLD ]; then
  echo "Disk usage is above $THRESHOLD% - Current: $USAGE%" | mail -s "Disk Alert on Server" admin@example.com
else
  echo "Disk usage is within acceptable limits. Current: $USAGE%"
fi
```

---

## ✅ Outcome

- Monitors root disk space in real time.
- Sends email alerts proactively to admin.
- Script scheduled via `cron` for daily checks.

---

## 🧰 Skills Demonstrated

- Bash scripting
- Disk space monitoring
- Use of `df`, `awk`, `sed`, and `cron`
- Basic email alert setup on Linux
