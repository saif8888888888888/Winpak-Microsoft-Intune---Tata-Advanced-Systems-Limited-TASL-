# Winpak-Microsoft-Intune---Tata-Advanced-Systems-Limited-TASL-
Microsoft Intune configurations for Winpak at Tata Advanced Systems Limited (TASL)
# Winpak-Microsoft-Intune Integration (TASL)

Automated security integration between Winpak physical access control and Microsoft Intune, developed for Tata Advanced Systems Limited (TASL).

## Overview

This solution automatically enforces device policies based on employee presence:
- **Punch IN** → Device moved to Restricted group → Camera disabled, personal apps blocked, etc.
- **Punch OUT** → Device restored to full access.

**Benefits**:
- Fully automated, zero manual intervention
- Policy application within 5 minutes
- Audit logging via Azure Monitor
- Supports iOS and Android (BYOD and corporate)

## Architecture

**Winpak API → Azure Function (PowerShell) → Graph API → Intune → Device**
## Prerequisites

- Azure subscription with Intune license
- Winpak API access (certificate-based authentication)
- Azure AD with `employeeId` attribute populated
- Test devices enrolled in Intune

## Deployment

1. Clone the repository  
   `git clone https://github.com/saifulla/Winpak-Microsoft-Intune-TASL.git`

2. Deploy Azure Function  
   `.\scripts\deploy-function.ps1 -ResourceGroupName "TASL-RG" -FunctionAppName "WinpakIntuneProcessor"`

3. Configure environment variables in Azure Function:
   - `RestrictedGroupId` – Intune group ID for restricted devices
   - `WinPakCertThumbprint` – certificate thumbprint for Winpak

4. Grant Graph API permissions to the function’s managed identity:
   - `Device.Read.All`
   - `GroupMember.ReadWrite.All`

5. Configure Winpak webhook to point to the Azure Function URL.

6. Import Intune policies from `intune-policies/` folder.

## Testing

Send a test punch event using the sample payload:

```bash
curl -X POST https://<function-app>.azurewebsites.net/api/ProcessWinpakPunch \
  -H "Content-Type: application/json" \
  -d @scripts/test-payload.json

Monitoring
All operations are logged to Azure Monitor. Alerts are configured for failures, throttling, and latency breaches.

Contact
Project Lead: Syed Saifulla H
Organization: Tata Advanced Systems Limited (TASL), Bengaluru
Email: saifulla.saif@icloud.com
