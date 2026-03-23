# osTicket-Homelab

Self-hosted help desk environment built on Windows Server 2022. Deployed and configured
from scratch as part of an ongoing IT homelab project.

## VM Architecture
```
VMware Workstation
│
├── WIN-AJ3IQ5KJNUB — Windows Server 2022
│   └── Active Directory, DNS, DHCP
│
└── OSTICKETMACHINE — Windows Server 2022
    ├── IIS
    ├── PHP
    ├── MySQL
    └── osTicket
```

## What it does
- Provides a self-hosted help desk portal for end users to submit tickets
- Provides a staff panel for IT to manage and resolve tickets
- Integrated with existing Active Directory environment
- Simulates a real help desk deployment

## Prerequisites
- Windows Server 2022
- VMware Workstation
- Existing Active Directory domain
- IIS with CGI enabled
- PHP 8.x Non-Thread Safe x64
- Visual C++ Redistributable 2022 x64
- URL Rewrite Module x64
- MySQL Community Server

## Installation
Full step-by-step installation notes including all errors encountered and their fixes
are documented in [SETUP.md](SETUP.md)

## Access
| Portal | URL |
|---|---|
| End user ticket portal | http://OSTICKETMACHINE/osticket |
| Staff login panel | http://OSTICKETMACHINE/osticket/scp |

## Configuration
| Setting | Value |
|---|---|
| Domain | hunterpractice.local |
| Web Server | IIS |
| PHP Version | 8.x Non-Thread Safe x64 |
| Database | MySQL — osticket |
| DNS | Managed by WIN-AJ3IQ5KJNUB |

## Ticket Scenarios
Simulated help desk scenarios practiced in this environment, documented with screenshots and full troubleshooting steps.
 
| # | Scenario | Description |
|---|---|---|
| 01 | [Account Lockout / Password Reset](scenarios/01-account-lockout/README.md) | User account locked after failed login attempts, resolved via Active Directory |
| 02 | [New Employee Onboarding](scenarios/02-new-employee/README.md) | New AD user account created, placed in correct OU with group memberships assigned |
| 03 | [Software Installation Request](scenarios/03-software-installation/README.md) | Software request triaged, approved, and installed via admin elevation through GPO restriction |
| 04 | [Printer Not Working](scenarios/04-printer-not-working/README.md) | Print Spooler crash diagnosed and resolved via error code 0x800706ba |
| 05 | [Cannot Access Network Share](scenarios/05-mapped-network-drive/README.md) | Z: drive inaccessible due to Deny permission on share, resolved after full connectivity diagnostic |

## Project Status
Base installation complete. Currently working on:
- Help desk ticket workflow simulations

## Acknowledgements
Created as a hands-on learning project following online resources,
with installation, troubleshooting, and configuration completed independently.
