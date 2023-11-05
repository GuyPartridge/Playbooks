# Prompt-User-Incident-Post-AdaptiveCard
author: Guy Partridge

Run this playbook on incidents which contains suspicious AAD activities. When a new incident is created, this playbook checks to ensure only one account entity exists to stop the SOC Team actioning multiple users. It then sends an email to the user to confirm if the activities were performed by them. If it was not the user then it posts an adaptive card in the SOC Microsoft Teams channel, including the potential risky user information given by Azure AD Identity Protection. The card offers to confirm the user as compromised or dismiss the compromised user in AADIP or to force a password reset and revoke the user's session. It also allows to configure the Azure Sentinel incident, tasks and tagging. A summary comment will be posted to document the action taken and user information. [Learn more about Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-identity-protection)

## Prerequisites
1. Using the riskyUsers API requires an Azure AD Premium P2 license. 
2. Have a managed identity which has the following permissions:
  
