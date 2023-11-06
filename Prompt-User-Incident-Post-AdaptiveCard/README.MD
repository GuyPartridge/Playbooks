# Prompt-User-Incident-Post-AdaptiveCard
author: Guy Partridge

Run this playbook on incidents which contains suspicious AAD activities. When a new incident is created, this playbook checks to ensure only one account entity exists to stop the SOC Team actioning multiple users. It then sends an email to the user to confirm if the activities were performed by them. If it was not the user then it posts an adaptive card in the SOC Microsoft Teams channel, including the potential risky user information given by Azure AD Identity Protection. The card offers to confirm the user as compromised or dismiss the compromised user in AADIP or to force a password reset and revoke the user's session. It also allows to configure the Azure Sentinel incident, tasks and tagging. A summary comment will be posted to document the action taken and user information. [Learn more about Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection)

##Flow
![0-automation](../Prompt-User-Incident-Post-AdaptiveCard/images/picture1.png)

## Prerequisites
1. Using the riskyUsers API requires an Azure AD Premium P2 license. 
2. Have a managed identity which has the following permissions:
a) User Admin role AAD Role is required to reset the passwords and sessions. Note that the user admin cannot reset privileged users (to do this assign the Privileged Role Administrator role.
b) The graph API permissions
"User.Read.All"
"User.ReadWrite.All"
"Directory.Read.All"
"Directory.ReadWrite.All"
"IdentityRiskyUser.ReadWrite.All"
"UserAuthenticationMethod.ReadWrite.All"

The permissions can be assigned using the Powershell code below.
If you don't have Azure AD PowerShell module, you will have to install it and connect to Azure AD PowerShell module. https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0

```powershell
$MIGuid = "<Enter your managed identity guid here>"
$MI = Get-AzureADServicePrincipal -ObjectId $MIGuid

$GraphAppId = "00000003-0000-0000-c000-000000000000"
$PermissionName1 = "User.Read.All"
$PermissionName2 = "User.ReadWrite.All"
$PermissionName3 = "Directory.Read.All"
$PermissionName4 = "Directory.ReadWrite.All"
$PermissionName5 = "IdentityRiskyUser.ReadWrite.All"
$PermissionName6 = "UserAuthenticationMethod.ReadWrite.All"
$GraphServicePrincipal = Get-AzureADServicePrincipal -Filter "appId eq '$GraphAppId'"
$AppRole1 = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName1 -and $_.AllowedMemberTypes -contains "Application"}
New-AzureAdServiceAppRoleAssignment -ObjectId $MI.ObjectId -PrincipalId $MI.ObjectId `
-ResourceId $GraphServicePrincipal.ObjectId -Id $AppRole1.Id
$AppRole2 = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName2 -and $_.AllowedMemberTypes -contains "Application"}
New-AzureAdServiceAppRoleAssignment -ObjectId $MI.ObjectId -PrincipalId $MI.ObjectId `
-ResourceId $GraphServicePrincipal.ObjectId -Id $AppRole2.Id
$AppRole3 = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName3 -and $_.AllowedMemberTypes -contains "Application"}
New-AzureAdServiceAppRoleAssignment -ObjectId $MI.ObjectId -PrincipalId $MI.ObjectId `
-ResourceId $GraphServicePrincipal.ObjectId -Id $AppRole3.Id
$AppRole4 = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName4 -and $_.AllowedMemberTypes -contains "Application"}
New-AzureAdServiceAppRoleAssignment -ObjectId $MI.ObjectId -PrincipalId $MI.ObjectId `
-ResourceId $GraphServicePrincipal.ObjectId -Id $AppRole4.Id
$AppRole5 = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName5 -and $_.AllowedMemberTypes -contains "Application"}
New-AzureAdServiceAppRoleAssignment -ObjectId $MI.ObjectId -PrincipalId $MI.ObjectId `
-ResourceId $GraphServicePrincipal.ObjectId -Id $AppRole5.Id
$AppRole6 = $GraphServicePrincipal.AppRoles | Where-Object {$_.Value -eq $PermissionName6 -and $_.AllowedMemberTypes -contains "Application"}
New-AzureAdServiceAppRoleAssignment -ObjectId $MI.ObjectId -PrincipalId $MI.ObjectId `
-ResourceId $GraphServicePrincipal.ObjectId -Id $AppRole6.Id
```
## Quick Deployment
**Deploy with incident trigger** (recommended)

After deployment, attach this playbook to an **automation rule** so it runs when the incident is created.

[Learn more about automation rules](https://docs.microsoft.com/azure/sentinel/automate-incident-handling-with-automation-rules#creating-and-managing-automation-rules)

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FGuyPartridge%2FPlaybooks%2Fmain%2FPrompt-User-Incident-Post-AdaptiveCard%2FPrompt-User-Post-Adaptive-Card.json)


