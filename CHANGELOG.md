# Changelog

All notable changes to this project will be documented here.

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
- Created WEB01 VM in VMware Workstation (Windows Server 2022, 60GB thin provisioned, 4GB RAM, 2 cores)
- Assigned static IP to WEB01 and pointed DNS to DC01 (hunterpractice.local)
- Joined WEB01 to existing hunterpractice.local domain
- Installed IIS with CGI enabled via Server Manager Add Roles and Features
