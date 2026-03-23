# Changelog

All notable changes to this project will be documented here.

---
 
## [2.0.0] - 2026-03-22
### Added
- Completed Tier 1 help desk scenario practice with full ticket lifecycle documentation
- Scenario 01: Account Lockout / Password Reset — triggered real domain lockout on COMP1, resolved via ADUC, documented in osTicket
- Scenario 02: New Employee Onboarding — created new AD user (smiller), placed in Reno OU, assigned group memberships, documented full onboarding checklist
- Scenario 03: Software Installation Request — simulated software request triage, confirmed GPO software restriction via UAC prompt, installed Notepad++ via admin elevation
- Scenario 04: Printer Not Working — diagnosed and resolved real error code 0x800706ba by stopping Print Spooler, clearing spool folder, and restarting service
- Created GitHub repo folder structure for all four scenarios with screenshots and README documentation
- Added canned responses to osTicket SCP for common resolutions
 
### Configuration
- Configured Account Lockout Policy in Default Domain Policy (threshold: 3 attempts, duration: manual unlock required)
- Confirmed GPO propagation to COMP1 via gpresult /r and net accounts
- Created and linked Software Restriction GPO to restrict standard users from installing software without admin elevation
- Added COMP1 computer object to USA → Computer OU in ADUC
 
---

## [1.0.0] - 2026-03-15
### Added
- Completed base osTicket installation and confirmed accessible at http://OSTICKETMACHINE/osticket
- Verified staff portal accessible at http://OSTICKETMACHINE/osticket/scp
- Completed post-install cleanup: removed setup directory, set ost-config.php to read only
- Added DNS A record for OSTICKETMACHINE on WIN-AJ3IQ5KJNUB to allow hostname resolution across domain

### Fixed
- Resolved "can't reach this page" error by adding OSTICKETMACHINE A record in DNS Manager on WIN-AJ3IQ5KJNUB

---

## [0.5.0] - 2026-03-15
### Added
- Ran osTicket web installer at http://OSTICKETMACHINE/osticket/setup
- Configured help desk name, admin credentials, and MySQL connection details

### Fixed
- Resolved mysqli access denied error by dropping and recreating osticket MySQL user
  with verified credentials, then re-entering in installer

---

## [0.4.0] - 2026-03-15
### Added
- Deployed osTicket upload folder to C:\inetpub\wwwroot\osticket
- Renamed ost-sampleconfig.php to ost-config.php
- Configured IIS_IUSRS modify permissions on ost-config.php

---

## [0.3.0] - 2026-03-15
### Added
- Installed MySQL Community Server, configured root password, left port at default 3306
- Created osticket database and dedicated osticket user with scoped privileges via MySQL CLI
- Installed URL Rewrite Module x64 from iis.net

### Fixed
- Resolved HTTP 500.19 error code 0x8007000d by installing URL Rewrite Module x64

---

## [0.2.0] - 2026-03-15
### Added
- Installed PHP 8.x Non-Thread Safe x64, extracted to C:\PHP
- Installed Visual C++ Redistributable 2022 x64
- Registered PHP with IIS via FastCGI handler mapping pointing to C:\PHP\php-cgi.exe
- Renamed php.ini-production to php.ini
- Uncommented extension_dir, required PHP extensions, and error_log in php.ini
- Restarted IIS to apply PHP configuration

### Fixed
- Resolved HTTP 500.0 PHP exited unexpectedly by installing Visual C++ Redistributable 2022 x64
- Resolved osTicket mysqli extension missing error by uncommenting extension_dir and
  required extensions in php.ini

---

## [0.1.0] - 2026-03-15
### Added
- Created OSTICKETMACHINE VM in VMware Workstation (Windows Server 2022, 40GB thin provisioned, 2GB RAM, 2 cores)
- Assigned static IP to OSTICKETMACHINE and pointed DNS to WIN-AJ3IQ5KJNUB (hunterpractice.local)
- Joined OSTICKETMACHINE to existing hunterpractice.local domain
- Installed IIS with CGI enabled via Server Manager Add Roles and Features
