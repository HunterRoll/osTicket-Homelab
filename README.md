# osTicket-Homelab

## VM Architecture
VMware Workstation
'''
│
├── DC01 (existing) — Windows Server 2022
│   └── Active Directory, DNS, DHCP
│
└── WEB01 (new VM) — Windows Server 2022 or Ubuntu Server
    ├── IIS or Apache
    ├── PHP
    ├── MySQL
    └── osTicket
'''
## First Few Steps
Fresh install of Windows Server 2022 on new VM in VMWare Workstation
Network Adapter Settings 
→ IPv4 Properties
→ Preferred DNS Server = DC01's static IP

Right click Start
→ System
→ Rename this PC (Advanced)
→ Change
→ Select Domain
→ Type your domain name (hunterpractice.local)
→ Enter domain admin credentials when prompted
→ Restart
