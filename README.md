<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

# osTicket Deployment on Azure (Windows Server + Windows 10)

## Overview

This project demonstrates how to deploy osTicket on a Windows Server virtual machine hosted in Microsoft Azure, using IIS, PHP, and MySQL. A Windows 10 virtual machine is used to simulate a client accessing the help desk system.

This lab demonstrates real-world enterprise concepts including web server configuration, database integration, client-server communication, and Azure virtual networking.

---

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines / Virtual Network)
- Windows Server 2022
- Windows 10
- Internet Information Services (IIS)
- PHP (FastCGI)
- MySQL Server
- Remote Desktop Protocol (RDP)
- HTTP / TCP-IP

---

## Operating Systems Used

- Windows Server 2022 Datacenter
- Windows 10 Pro

---

## Architecture

- Azure Virtual Network  

- Windows Server 2022 VM  

- IIS Web Server  

- PHP Runtime  

- MySQL Database  

- osTicket Application  

- Windows 10 VM  

- Web Browser Client Access  

---

## Deployment and Configuration Steps

### Step 1: Create Azure Virtual Machines

#### Windows Server VM

<img width="796" height="359" alt="image" src="https://github.com/user-attachments/assets/94294fce-41a3-4187-bb1d-662ba41c92e0" />

- Image: Windows Server 2022 Datacenter  
- Size: Standard B2s  
- Open inbound ports:
  - 3389 (RDP)
  - 80 (HTTP)  
- Place inside a Virtual Network  

#### Windows 10 VM

- Image: Windows 10 Pro  
- Place in the same Virtual Network  
- Allow RDP (3389)  

---

### Step 2: Install IIS (Web Server)

1. RDP into Windows Server VM.
2. Open Server Manager.
3. Click Add Roles and Features.
4. Select:
   - Web Server (IIS)
   - CGI (under Application Development)
5. Install and verify by browsing: http://localhost
6. The IIS default page should load.

---

### Step 3: Install PHP

1. Download PHP for Windows from the official PHP website: https://www.php.net/
2. Extract files to: C:\PHP
3. Open IIS Manager.
4. Go to Handler Mappings.
5. Click Add Module Mapping.

Configure:
Request Path: *.php
Module: FastCgiModule
Executable: C:\PHP\php-cgi.exe
Name: PHP_FastCGI
Restart IIS: iisrestart

---

### Step 4: Install MySQL

1. Download MySQL Installer.
2. Install MySQL Server.
3. Configure a strong root password.
4. Use default port 3306.

---

### Step 5: Create osTicket Database

---

### Step 6: Deploy osTicket

1. Download the latest osTicket release from GitHub.
https://github.com/osTicket/osTicket/releases

2. Extract the contents.

3. Move the upload folder to: C:\inetpub\wwwroot\osticket

4. Rename: include\ost-sampleconfig.php
to: include\ost-config.php

---

### Step 7: Configure NTFS Permissions

1. Right-click the osticket folder.

2. Select Properties → Security.

3. Add: IIS_IUSRS

4. Grant: Modify
Read & Execute
Write

---

### Step 8: Enable Required PHP Extensions

1. Open:
C:\PHP\php.ini

2. Uncomment the following lines:
extension=php_imap
extension=php_intl
extension=php_mbstring
extension=php_mysqli

3. Restart IIS:
iisreset

---

### Step 9: Complete Web Installation

1. From either the Windows Server VM or Windows 10 VM, open:
http://<server-private-ip>/osticket

2. Enter database information:
Database Name: osticket
Username: osticketuser
Password: StrongPassword123!

3. Create an admin account and complete installation.

---

### Step 10: Post-Installation Security

1. Delete the setup directory:
C:\inetpub\wwwroot\osticket\setup

2. Set configuration file to read-only:

C:\inetpub\wwwroot\osticket\include\ost-config.php

### Testing from Windows 10 Client

1. RDP into Windows 10 VM.

2. Open a web browser.

3. Navigate to: http://<server-private-ip>/osticket

4. Submit a test ticket.

5. Confirm ticket appears in admin panel.

---

### Skills Demonstrated

- Azure VM provisioning

- Virtual Network configuration

- Windows Server administration

- IIS configuration

- PHP FastCGI configuration

- MySQL database setup

- NTFS permission management

- Client-server communication

- Web application deployment

---

### Outcome

Successfully deployed and configured osTicket in a Windows Server environment hosted in Microsoft Azure. Verified functionality using a Windows 10 client within the same virtual network.
