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

Azure Virtual Network  

Windows Server 2022 VM  
IIS Web Server  
PHP Runtime  
MySQL Database  
osTicket Application  

Windows 10 VM  
Web Browser Client Access  

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

1. Download PHP for Windows from the official PHP website.
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
