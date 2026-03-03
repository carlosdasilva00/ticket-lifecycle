<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

osTicket Deployment on Azure (Windows Server + Windows 10 Client)
Project Overview

This project demonstrates deploying osTicket on a Windows Server VM hosted in Microsoft Azure.

The environment simulates a real-world enterprise setup:

Windows Server 2022 (Web Server)

IIS (Microsoft Web Server)

PHP (FastCGI)

MySQL Database

Windows 10 Client VM (User Simulation)

Architecture
Azure Virtual Network
│
├── Windows Server 2022 VM
│     ├── IIS (Web Server)
│     ├── PHP
│     ├── MySQL
│     └── osTicket
│
└── Windows 10 VM (Client)
      └── Web Browser → Access Helpdesk
1. Azure Virtual Machine Deployment
Create Windows Server VM

Configuration:

Image: Windows Server 2022 Datacenter

Size: Standard B2s

Inbound ports:

3389 (RDP)

80 (HTTP)

Place inside a Virtual Network

Create Windows 10 Client VM

Image: Windows 10 Pro

Same Virtual Network as Server

Allow RDP (3389)

This VM simulates an internal user accessing the helpdesk system.

2. Install IIS (Web Server Role)

Log into Windows Server via RDP.

Open:

Server Manager → Add Roles and Features

Select:

Web Server (IIS)

CGI (under Application Development)

Verify installation:

http://localhost

The default IIS page should load.

3. Install PHP (FastCGI Configuration)

Download PHP from:

https://windows.php.net

Extract to:

C:\PHP
Configure IIS for PHP

Open IIS Manager

Navigate to: Handler Mappings

Select: Add Module Mapping

Use the following configuration:

Request Path: *.php
Module: FastCgiModule
Executable: C:\PHP\php-cgi.exe
Name: PHP_via_FastCGI

Restart IIS:

iisreset
4. Install MySQL

Download MySQL Installer:

https://dev.mysql.com

Install:

MySQL Server

Configure root password

Port: 3306 (default)

5. Create Database for osTicket

Open MySQL Command Line Client and run:

CREATE DATABASE osticket;

CREATE USER 'osticketuser'@'localhost'
IDENTIFIED BY 'StrongPassword123!';

GRANT ALL PRIVILEGES ON osticket.*
TO 'osticketuser'@'localhost';

FLUSH PRIVILEGES;
6. Deploy osTicket

Download latest release:

https://github.com/osTicket/osTicket

Extract the contents.

Move the upload folder to:

C:\inetpub\wwwroot\osticket

Rename the configuration file:

include\ost-sampleconfig.php

to:

include\ost-config.php
7. Configure NTFS Permissions

Right-click the osticket folder

Select Properties → Security

Add:

IIS_IUSRS

Grant the following permissions:

Modify

Read & Execute

Write

8. Enable Required PHP Extensions

Open:

C:\PHP\php.ini

Uncomment (remove ;):

extension=php_imap
extension=php_intl
extension=php_mbstring
extension=php_mysqli

Restart IIS:

iisreset
9. Complete Web Installation

From the Windows Server VM or Windows 10 Client VM, open:

http://<server-private-ip>/osticket

Enter:

Database Name: osticket
Username: osticketuser
Password: StrongPassword123!

Create the admin account and complete installation.

10. Post-Installation Security

Delete setup directory:

C:\inetpub\wwwroot\osticket\setup

Set configuration file to Read-Only:

C:\inetpub\wwwroot\osticket\include\ost-config.php
Technologies Used

Cloud Platform: Microsoft Azure

Server OS: Windows Server 2022

Client OS: Windows 10

Web Server: IIS

Scripting Language: PHP

Database: MySQL

Application: osTicket

Protocols: HTTP, TCP/IP, RDP

Skills Demonstrated

Azure VM provisioning

Windows Server administration

IIS configuration and FastCGI setup

MySQL database configuration

NTFS permission management

Client-server networking within Azure VNet

Web application deployment in Windows environment
