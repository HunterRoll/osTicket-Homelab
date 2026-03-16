# Changelog

All notable changes to this project will be documented here.

---

## [1.0.0] - 2026-03-15
### Added
- Completed base osTicket installation and confirmed accessible at http://OSTICKETMACHINE/osticket
- Verified staff portal accessible at http://OSTICKETMACHINE/osticket/scp
- Completed post-install cleanup: removed setup directory, set ost-config.php to read only
- Added DNS A record for OSTICKETMACHINE on DC01 to allow hostname resolution across domain

### Fixed
- Resolved "can't reach this page" error by adding OSTICKETMACHINE A record in DNS Manager on DC01

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
- Assigned static IP to OSTICKETMACHINE and pointed DNS to DC01 (hunterpractice.local)
- Joined OSTICKETMACHINE to existing hunterpractice.local domain
- Installed IIS with CGI enabled via Server Manager Add Roles and Features
