# Active-Directory-Automation-Lab
Import-Module ActiveDirectory

$CSVPath = "C:\ServerLogs\NewHires.csv"

if (-not (Test-Path $CSVPath)) {
    Write-Error "HR CSV file not found at $CSVPath" -ForegroundColor Red
    exit
}

# Pull the active domain suffix (e.g., azure.local) automatically
$DNSRoot = (Get-ADDomain).DNSRoot
$Users = Import-Csv -Path $CSVPath

foreach ($User in $Users) {
    # Define Corporate Username Standards
    $SAMAccountName = ($User.FirstName.Substring(0,1) + $User.LastName).ToLower()
    $UserPrincipalName = "$SAMAccountName@$DNSRoot"
    $DisplayName = "$($User.LastName), $($User.FirstName)"
    $Email = "$SAMAccountName@company.ie"
    $Password = ConvertTo-SecureString "TempWelcome2026!" -AsPlainText -Force
    
    Write-Host "Evaluating Identity: $SAMAccountName..." -ForegroundColor Cyan

    # Check if user already exists inside Active Directory
    $ExistingUser = Get-ADUser -Filter "SamAccountName -eq '$SAMAccountName'" -ErrorAction SilentlyContinue

    if ($ExistingUser) {
        Write-Host " -> SKIPPED: $SAMAccountName already exists in Active Directory." -ForegroundColor Yellow
    } else {
        # Create Native Active Directory User Account
        New-ADUser -Name $DisplayName `
                   -SamAccountName $SAMAccountName `
                   -UserPrincipalName $UserPrincipalName `
                   -DisplayName $DisplayName `
                   -GivenName $User.FirstName `
                   -Surname $User.LastName `
                   -Department $User.Department `
                   -Title $User.Title `
                   -Office $User.Office `
                   -EmailAddress $Email `
                   -AccountPassword $Password `
                   -ChangePasswordAtLogon $true `
                   -Enabled $true

        Write-Host " -> SUCCESS: Created AD Account for $DisplayName" -ForegroundColor Green
    }
}
