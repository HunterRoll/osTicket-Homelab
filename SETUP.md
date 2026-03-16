# osTicket Setup Guide

Full installation notes for deploying osTicket on Windows Server 2022,
including all errors encountered and their resolutions.

---

## Environment
- **WIN-AJ3IQ5KJNUB** — Windows Server 2022, existing domain controller (hunterpractice.local)
- **OSTICKETMACHINE** — Windows Server 2022, new VM hosting osTicket

---

## Step 1 — Create the OSTICKETMACHINE VM in VMware
**Settings used:**
- OS: Windows Server 2022
- Disk: 40GB (thin provisioned)
- RAM: 2GB
- CPU: 2 cores
- Network: same virtual network as WIN-AJ3IQ5KJNUB

Install Windows Server 2022 normally through the VMware setup wizard.

---

## Step 2 — Configure Networking and Join the Domain
1. Set a **static IP** on OSTICKETMACHINE
2. Set **Preferred DNS** to WIN-AJ3IQ5KJNUB's static IP:
```
Network Adapter Settings
→ IPv4 Properties
→ Preferred DNS Server = WIN-AJ3IQ5KJNUB's IP
```
3. Join the domain:
```
Right click Start
→ System
→ Rename this PC (Advanced)
→ Change
→ Select Domain
→ Type: hunterpractice.local
→ Enter domain admin credentials
→ Restart
```
4. After restart, verify OSTICKETMACHINE appears in **Active Directory Users and Computers**
   on WIN-AJ3IQ5KJNUB under the Computers container

---

## Step 3 — Install IIS
In Server Manager:
```
Manage → Add Roles and Features
→ Role-based installation
→ Web Server (IIS)
→ Role Services → Application Development → CGI ✅ (critical, do not skip)
→ Install
```

---

## Step 4 — Install PHP
1. Download **PHP 8.x Non-Thread Safe x64** from windows.php.net
2. Create folder `C:\PHP`
3. Extract zip contents to `C:\PHP`
4. Install **Visual C++ Redistributable 2022 x64** from:
```
aka.ms/vs/17/release/vc_redist.x64.exe
```
5. Register PHP with IIS:
```
IIS Manager → Handler Mappings → Add Module Mapping
Request path: *.php
Module: FastCgiModule
Executable: C:\PHP\php-cgi.exe
Name: PHP
```

### Configure php.ini
1. In `C:\PHP` rename **php.ini-production** to **php.ini**
2. Open php.ini in Notepad and uncomment/edit these lines (remove the semicolon):
```ini
extension_dir = "C:\PHP\ext"

extension=mysqli
extension=gd
extension=gettext
extension=mbstring
extension=openssl
extension=intl

error_log = "C:\PHP\php_errors.log"
```
3. Save php.ini
4. Restart IIS:
```
IIS Manager → Click server name → Restart
```

---

## Step 5 — Install URL Rewrite Module
1. Download the **x64 installer** from:
```
iis.net/downloads/microsoft/url-rewrite
```
2. Run the installer
3. Restart IIS after installing

---

## Step 6 — Install MySQL
1. Download **MySQL Community Server** from mysql.com — choose **Server Only**
2. Run installer, set a strong root password and write it down
3. Leave port as default **3306**

### Create the osTicket Database
Open MySQL Command Line Client, enter root password, then run:
```sql
CREATE DATABASE osticket;
CREATE USER 'osticket'@'localhost' IDENTIFIED BY 'yourpassword';
GRANT ALL PRIVILEGES ON osticket.* TO 'osticket'@'localhost';
FLUSH PRIVILEGES;
```
Verify it worked:
```sql
SHOW DATABASES;
```
`osticket` should appear in the list.

---

## Step 7 — Deploy osTicket
1. Download latest release from osticket.com
2. Extract the zip to a temporary location
3. Copy the **upload** folder to `C:\inetpub\wwwroot`
4. Rename the folder from **upload** to **osticket**
5. Navigate to `C:\inetpub\wwwroot\osticket\include`
6. Rename **ost-sampleconfig.php** to **ost-config.php**
7. Set permissions on ost-config.php:
```
Right click ost-config.php
→ Properties → Security
→ Add IIS_IUSRS with Modify permissions
```

---

## Step 8 — Run the Web Installer
Visit from any domain machine:
```
http://OSTICKETMACHINE/osticket/setup
```
Fill in:
| Field | Value |
|---|---|
| Help desk name | HunterPractice IT Help Desk |
| Default email | helpdesk@hunterpractice.local |
| Admin email | your admin email |
| Admin password | your admin password |
| MySQL Database | osticket |
| MySQL Username | osticket |
| MySQL Password | whatever you set in Step 6 |
| MySQL Hostname | localhost |

---

## Step 9 — Post Install Cleanup
**Delete the setup folder:**
```
C:\inetpub\wwwroot\osticket
→ Right click setup folder → Delete
```
**Set ost-config.php to Read Only:**
```
C:\inetpub\wwwroot\osticket\include
→ Right click ost-config.php
→ Properties → Check Read Only → Apply
```

---

## Step 10 — Add DNS Record for OSTICKETMACHINE
On WIN-AJ3IQ5KJNUB:
```
DNS Manager
→ Forward Lookup Zones
→ hunterpractice.local
→ Right click → New Host (A record)
→ Name: OSTICKETMACHINE
→ IP: OSTICKETMACHINE's static IP
→ Add Host
```

---

## Verify Installation
| Portal | URL |
|---|---|
| End user ticket portal | http://OSTICKETMACHINE/osticket |
| Staff login panel | http://OSTICKETMACHINE/osticket/scp |

---

## Errors Encountered and Fixes

### HTTP 500.19 — Error Code 0x8007000d
- **Cause:** URL Rewrite Module not installed. osTicket's web.config references it
  and IIS throws 0x8007000d when it cannot find it
- **Fix:** Downloaded and installed URL Rewrite Module x64 from iis.net, restarted IIS

### HTTP 500.0 — PHP Exited Unexpectedly
- **Cause:** Visual C++ Redistributable not installed. PHP on Windows requires it to run
- **Fix:** Downloaded and installed VC++ Redistributable 2022 x64, restarted IIS

### osTicket Installer Showing mysqli Extension Missing
- **Cause:** php.ini did not exist and all extensions were commented out by default
- **Fix:** Renamed php.ini-production to php.ini, uncommented extension_dir and all
  required extensions, restarted IIS

### mysqli Access Denied for osticket@localhost
- **Cause:** Password mismatch between MySQL user and credentials entered in installer
- **Fix:** Dropped and recreated the osticket MySQL user with a verified password:
```sql
DROP USER 'osticket'@'localhost';
CREATE USER 'osticket'@'localhost' IDENTIFIED BY 'yournewpassword';
GRANT ALL PRIVILEGES ON osticket.* TO 'osticket'@'localhost';
FLUSH PRIVILEGES;
```
Then re-entered credentials in the osTicket installer

### Hmmm, Can't Reach This Page
- **Cause:** No DNS A record existed for OSTICKETMACHINE so machines could not resolve the hostname
- **Fix:** Added A record for OSTICKETMACHINE in DNS Manager on WIN-AJ3IQ5KJNUB pointing to OSTICKETMACHINE's static IP
