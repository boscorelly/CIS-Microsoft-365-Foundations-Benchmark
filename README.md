# CIS Microsoft 365 Foundations Benchmark v6.0.0 - Automated Compliance Checker

[![PowerShell Gallery](https://img.shields.io/powershellgallery/v/CIS-M365-Benchmark.svg)](https://www.powershellgallery.com/packages/CIS-M365-Benchmark)
[![PowerShell Gallery Downloads](https://img.shields.io/powershellgallery/dt/CIS-M365-Benchmark.svg)](https://www.powershellgallery.com/packages/CIS-M365-Benchmark)
[![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B%20%7C%207%2B-blue.svg)](https://github.com/PowerShell/PowerShell)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![CIS Benchmark](https://img.shields.io/badge/CIS%20Benchmark-v6.0.0-orange.svg)](https://www.cisecurity.org/)
[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-Support-yellow.svg)](https://buymeacoffee.com/mohammedsiddiqui)
[![PowerShellNerd Profile](https://img.shields.io/badge/PowerShellNerd-Profile-purple.svg)](https://powershellnerd.com/profile/mohammedsiddiqui)

A comprehensive PowerShell module that audits your Microsoft 365 environment against **all 140 CIS Microsoft 365 Foundations Benchmark v6.0.0 controls** and generates detailed HTML and CSV compliance reports.

## What's New in v5.1.0

**v5.1.0 - ExcludeSections, Disconnect, Scope Fixes & Sorting**
- **`-ExcludeSections` parameter** - Skip sections like Intune, Teams, or PowerBI (great for hybrid environments)
- **`Disconnect-CISM365Benchmark`** - Cleanly disconnect all M365 sessions (Graph, Exchange, SPO, Teams, IPPS)
- **Fixed controls 1.3.4 & 1.3.5** - Added missing `OrgSettings-AppsAndServices.Read.All` and `OrgSettings-Forms.Read.All` Graph scopes
- **Fixed report sorting** - Controls like 9.1.10 now sort correctly after 9.1.9 (not before 9.1.2)
- **Version alignment** - All files now consistently report v5.1.0

<details>
<summary><strong>v5.0.0 - 95% Automation, MSAL Auth, Fabric API & Theme Toggle</strong></summary>

- **95% Automation** - 133 of 140 controls now fully automated (up from 92)
- **MSAL-based authentication** - Graph + SharePoint tokens acquired at connect time, fewer login prompts
- **All 12 Power BI/Fabric controls automated** - Native Fabric Admin API integration via MSAL token
- **Security & Compliance Center integration** - Purview DLP and retention checks automated via `Connect-IPPSSession`
- **MSAL assembly conflict solved** - Isolated subprocess for Fabric token acquisition
- **Light/Dark theme toggle** - HTML report now includes a theme switcher with localStorage persistence
- **80% fewer API calls** - Intelligent caching across all sections
- **MSOnline module fully retired** - 100% Microsoft Graph API
- **Print override** - HTML reports force light theme when printing
</details>

## Features

- **140 Compliance Controls** across all M365 services
- **95% Fully Automated** - 133 controls run automatically via Microsoft Graph API and native REST APIs
- **Light/Dark Theme Toggle** - HTML reports with switchable themes and print-friendly output
- **Zero-Parameter Authentication** - `Connect-CISM365Benchmark` with MSAL-based token management
- **Dual Report Format** - Professional HTML and CSV reports with floating action buttons
- **Profile-based Filtering** - Check L1, L2, or All controls
- **Secure Authentication** - Modern OAuth 2.0 with MSAL token caching
- **Read-Only Assessment** - No changes to your environment
- **Actionable Remediation** - Each failed check includes specific remediation steps
- **PowerShell 5.1 & 7+ Compatible** - Works on Windows PowerShell and PowerShell Core
- **Cached API Calls** - 80% fewer API calls with intelligent caching

## Automation Coverage

| Category | Total Controls | Automated | Manual | Coverage |
|----------|---------------|-----------|--------|----------|
| **Section 1: M365 Admin** | 15 | 13 | 2 | 87% |
| **Section 2: M365 Defender** | 20 | 18 | 2 | 90% |
| **Section 3: Purview** | 4 | 4 | 0 | 100% |
| **Section 4: Intune** | 2 | 2 | 0 | 100% |
| **Section 5: Entra ID** | 45 | 43 | 2 | 96% |
| **Section 6: Exchange** | 12 | 12 | 0 | 100% |
| **Section 7: SharePoint** | 13 | 12 | 1 | 92% |
| **Section 8: Teams** | 17 | 17 | 0 | 100% |
| **Section 9: Power BI** | 12 | 12 | 0 | 100% |
| **TOTAL** | **140** | **133** | **7** | **95%** |

## Installation

```powershell
# Install from PowerShell Gallery
Install-Module -Name CIS-M365-Benchmark -Scope CurrentUser

# Update to latest version
Update-Module -Name CIS-M365-Benchmark
```

## Prerequisites

### Required PowerShell Modules

The following modules are **automatically installed** when you first use the module:

| Module | Purpose |
|--------|---------|
| `Microsoft.Graph` (v2.0+) | Entra ID, Conditional Access, PIM, Authentication Methods |
| `ExchangeOnlineManagement` (v3.1+) | Exchange Online configuration checks |
| `Microsoft.Online.SharePoint.PowerShell` | SharePoint Online tenant configuration checks |
| `MicrosoftTeams` | Teams meeting, messaging, and federation policies |

### Required Permissions

Your account needs the following permissions:

**Microsoft Graph API:**
- `Directory.Read.All`
- `Policy.Read.All`
- `AuditLog.Read.All`
- `UserAuthenticationMethod.Read.All`
- `IdentityRiskyUser.Read.All`
- `Application.Read.All`
- `Organization.Read.All`
- `User.Read.All`
- `Group.Read.All`
- `RoleManagement.Read.All`
- `Reports.Read.All`
- `DeviceManagementConfiguration.Read.All`
- `DeviceManagementServiceConfig.Read.All`
- `OrgSettings-AppsAndServices.Read.All`
- `OrgSettings-Forms.Read.All`

**Exchange Online:**
- View-Only Organization Management or higher

**SharePoint Online:**
- SharePoint Administrator or Global Administrator

**Microsoft Teams:**
- Teams Administrator or Global Administrator

## Usage

```powershell
# Quick start - 3 steps
Import-Module CIS-M365-Benchmark
Connect-CISM365Benchmark
Invoke-CISM365Benchmark

# Specify tenant manually
Invoke-CISM365Benchmark `
    -TenantDomain "contoso.onmicrosoft.com" `
    -SharePointAdminUrl "https://contoso-admin.sharepoint.com"

# Check only L1 or L2 controls
Invoke-CISM365Benchmark -ProfileLevel "L1"
Invoke-CISM365Benchmark -ProfileLevel "L2"

# Exclude specific sections (useful for hybrid environments)
Invoke-CISM365Benchmark -ExcludeSections Intune
Invoke-CISM365Benchmark -ExcludeSections Intune, Teams, PowerBI

# Custom output directory
Invoke-CISM365Benchmark -OutputPath "C:\CIS-Reports"

# Device code authentication (headless/remote sessions)
Connect-CISM365Benchmark -UseDeviceCode
Invoke-CISM365Benchmark

# Full example with all parameters
Invoke-CISM365Benchmark `
    -TenantDomain "contoso.onmicrosoft.com" `
    -SharePointAdminUrl "https://contoso-admin.sharepoint.com" `
    -ProfileLevel "All" `
    -OutputPath "C:\Security\CIS-Reports" `
    -ExcludeSections Intune `
    -Verbose

# Disconnect all M365 sessions when done
Disconnect-CISM365Benchmark

# Look up a specific control
Get-CISM365BenchmarkControl -ControlNumber "5.2.2.1"

# Check prerequisites and module versions
Test-CISM365BenchmarkPrerequisites

# Module info
Get-CISM365BenchmarkInfo
```

### Available ExcludeSections Values

| Value | Section |
|-------|---------|
| `AdminCenter` | Section 1: Microsoft 365 Admin Center |
| `Defender` | Section 2: Microsoft 365 Defender |
| `Purview` | Section 3: Microsoft Purview |
| `Intune` | Section 4: Microsoft Intune |
| `EntraID` | Section 5: Microsoft Entra ID |
| `Exchange` | Section 6: Exchange Online |
| `SharePoint` | Section 7: SharePoint Online |
| `Teams` | Section 8: Microsoft Teams |
| `PowerBI` | Section 9: Power BI |

## Output Reports

### HTML Report
- **File**: `CIS-M365-Compliance-Report_YYYYMMDD_HHMMSS.html`
- Professional report with **light/dark theme toggle** (preference saved to localStorage)
- Summary dashboard with progress bars, L1/L2 breakdown, and compliance statistics
- Filterable results table with search, remediation steps for each failed control
- Print-friendly output (automatically forces light theme)

### CSV Report
- **File**: `CIS-M365-Compliance-Report_YYYYMMDD_HHMMSS.csv`
- Import into Excel for tracking over time or further analysis

### Audit Log
- **File**: `CIS-M365-Audit_YYYYMMDD_HHMMSS.log`
- Timestamped log of every check result for compliance evidence

## Troubleshooting

**Issue: "Connect-CISM365Benchmark is not recognized"**
- Install the latest version: `Install-Module -Name CIS-M365-Benchmark -Scope CurrentUser -Force`

**Issue: Authentication browser window doesn't open**
- Use device code authentication: `Connect-CISM365Benchmark -UseDeviceCode`

**Issue: Multiple sign-in prompts**
- Normal. Each M365 service (Graph, Exchange, SharePoint, Teams) may prompt separately.

**Issue: SPO authentication fails on PowerShell 7+**
- The module handles this automatically. If issues persist, try Windows PowerShell 5.1.

**Issue: PIM or Identity Governance errors**
- Ensure PIM is licensed. Controls 5.3.4/5.3.5 require Entra ID P2.

## Security Considerations

- **Read-Only**: Script only reads configuration, never modifies settings
- **Secure Auth**: Uses OAuth 2.0 modern authentication
- **No Credentials Stored**: Authentication tokens are session-based only
- **No MSOL Dependency**: Fully migrated to Microsoft Graph API
- **Audit Trail**: All checks are logged with timestamps
- **Sensitive Data**: Reports may contain tenant configuration details - store securely

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## References

- [CIS Microsoft 365 Foundations Benchmark v6.0.0](https://www.cisecurity.org/benchmark/microsoft_365)
- [Microsoft Graph API Documentation](https://docs.microsoft.com/en-us/graph/)
- [Microsoft 365 Security Best Practices](https://docs.microsoft.com/en-us/microsoft-365/security/)

## Author

**Mohammed Siddiqui**
- LinkedIn: [Let's Chat!](https://www.linkedin.com/in/mohammedsiddiqui6872/)
- Profile: [PowerShellNerd Profile](https://powershellnerd.com/profile/mohammedsiddiqui)

## Acknowledgments

- Thanks to ITEngineer-0815, M0nk3yOo, ozsaid, boscorelly, Josip-I, albert-widjaja, valhalla94, and Mateusz Jagiello for their contributions and issue reports
- Special Thanks to **Mateusz Jagiello** For his relentless checks.

## Support

For issues, questions, or suggestions:
- [Open an Issue](https://github.com/mohammedsiddiqui6872/CIS-Microsoft-365-Foundations-Benchmark/issues)
- [Start a Discussion](https://github.com/mohammedsiddiqui6872/CIS-Microsoft-365-Foundations-Benchmark/discussions)

---

**If you find this tool helpful, please consider giving it a star!**

**Disclaimer**: This toolkit is not affiliated with or endorsed by Microsoft Corporation or CIS (Center for Internet Security). This script is provided as-is for compliance assessment purposes. Always test in a non-production environment first.
