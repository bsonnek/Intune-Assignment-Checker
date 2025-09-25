Here is a clean README you can drop into the project.

# Intune Assignment Checker

Generate a human-readable HTML report of Intune configuration assignments. The script is designed to run from `C:\Intune-Assignment-Checker`, and it will save reports to `C:\Intune-Assignment-Checker\Reports`.

## Requirements

* Windows with PowerShell 5.1 or PowerShell 7
* Internet access for Microsoft Graph auth
* Account with permission to read Intune configuration and assignments
  (for example, Intune Administrator or custom role with read access)

## Install Microsoft Graph Auth module

```powershell
Install-Module Microsoft.Graph.Authentication -Scope CurrentUser
```

If prompted to trust the repository, choose Yes.

## Recommended folder layout

```
C:\
  Intune-Assignment-Checker\
    IntuneAssignmentChecker.ps1
    README.md
    Reports\   (created automatically on first run)
```

Copy this project folder to `C:\` on the target device.

## Retrieve your Tenant ID

1. Go to [https://entra.microsoft.com](https://entra.microsoft.com)
2. Azure Active Directory, Overview, copy the Tenant ID (GUID)

## Quick start

Open PowerShell as Administrator, then run:

```powershell
cd C:\Intune-Assignment-Checker
.\IntuneAssignmentChecker.ps1 -TenantId '1111-1111-1111-1111-1111111111' -GenerateHTMLReport
```

You will be prompted to sign in. The report opens in your default browser after it is generated.

## What the script does

* Authenticates to Microsoft Graph using your Tenant ID
* Collects Intune configuration objects and their group assignments
* Builds an HTML report with summary tables and detail sections
* Saves the report to `C:\Intune-Assignment-Checker\Reports` using a timestamped name
  for example, `IntuneAssignmentReport_20250925_1325.html`

## Common options

Run interactively, generates an HTML report:

```powershell
.\IntuneAssignmentChecker.ps1 -TenantId 'YOUR-TENANT-GUID' -GenerateHTMLReport
```

Sign in prompt only, no report:

```powershell
.\IntuneAssignmentChecker.ps1 -TenantId 'YOUR-TENANT-GUID'
```

If you renamed the patched file and want to keep both scripts, just call the patched one:

```powershell
.\IntuneAssignmentChecker_patched.ps1 -TenantId 'YOUR-TENANT-GUID' -GenerateHTMLReport
```

## Execution policy tip

If your environment blocks script execution, use a temporary policy for the current PowerShell process:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

Then run the script as shown above.

## Updating the Graph module

```powershell
Update-Module Microsoft.Graph.Authentication
```

## Troubleshooting

* Sign in loop or auth errors
  Close all PowerShell windows, reopen as Administrator, run the install and script commands again.
  You can also clear stored tokens by closing all PowerShell sessions.

* Missing data in the report
  Confirm your account has Intune read permissions. Try an account with Intune Administrator.

* Report did not appear
  Check `C:\Intune-Assignment-Checker\Reports` for the latest HTML file.
  Verify that antivirus or endpoint protection did not block file creation.

* Running from a different folder
  Always `cd C:\Intune-Assignment-Checker` before you run the script, or create a shortcut that starts in this folder.

## Privacy note

The script reads configuration and assignment metadata from your tenant. Data is not sent anywhere except Microsoft Graph. Reports are stored locally on the device at `C:\Intune-Assignment-Checker\Reports`. Delete HTML files when finished if needed.
