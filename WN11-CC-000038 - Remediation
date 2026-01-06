<#
.SYNOPSIS
    This PowerShell script disables WDigest Authentication to prevent plaintext credentials from being stored in LSASS.

.NOTES
    Author          : Ayca Sanli
    LinkedIn        : https://www.linkedin.com/in/ayca-sanli-0489aa163/
    GitHub          : https://github.com/aycasanli8
    Date Created    : 2026/01/05
    Last Modified   : 2026/01/05
    Version         : 1.0
    CVEs            : CVE-2011-1935
    Plugin IDs      : N/A
    STIG-ID         : WN11-CC-000038

.TESTED ON
    Date(s) Tested  : 2026/01/05
    Tested By       : Ayca Sanli
    Systems Tested  : Windows 11 Pro (24H2)
    PowerShell Ver. : 5.1, 7.4

.USAGE
    Run this script as Administrator.
    Example syntax:
    PS C:\> .\remediate_WN11-CC-000038.ps1 
#>

# Define the Registry path and value details
$regPath = "HKLM:\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest"
$regName = "UseLogonCredential"
$desiredValue = 0

Write-Host "------------------------------------------------------------" -ForegroundColor Cyan
Write-Host "Starting Remediation for STIG ID: WN11-CC-000038" -ForegroundColor Cyan
Write-Host "Goal: Disable WDigest Authentication (Prevent Credential Dumping)" -ForegroundColor Gray
Write-Host "------------------------------------------------------------" -ForegroundColor Cyan

try {
    # 1. Check if the registry key path exists, create if it doesn't
    if (-not (Test-Path $regPath)) {
        Write-Host "[INFO] Registry path not found. Creating path: $regPath" -ForegroundColor Yellow
        New-Item -Path $regPath -Force | Out-Null
    }

    # 2. Check current value
    $currentValue = (Get-ItemProperty -Path $regPath -Name $regName -ErrorAction SilentlyContinue).$regName

    if ($currentValue -eq $desiredValue) {
        Write-Host "[OK] System is already compliant. '$regName' is set to $desiredValue." -ForegroundColor Green
    }
    else {
        # 3. Apply the fix
        Write-Host "[INFO] Remediating... Setting '$regName' to $desiredValue." -ForegroundColor Yellow
        Set-ItemProperty -Path $regPath -Name $regName -Value $desiredValue -Type DWord -ErrorAction Stop
        
        # 4. Verify the fix
        $newValue = (Get-ItemProperty -Path $regPath -Name $regName).$regName
        if ($newValue -eq $desiredValue) {
            Write-Host "[SUCCESS] Remediation applied successfully." -ForegroundColor Green
        } else {
            Write-Host "[ERROR] Failed to verify the registry change." -ForegroundColor Red
        }
    }
}
catch {
    Write-Host "[ERROR] An unexpected error occurred: $_" -ForegroundColor Red
}

Write-Host "------------------------------------------------------------" -ForegroundColor Cyan
