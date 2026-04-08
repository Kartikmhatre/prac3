# PRACTICAL 3
## Install and Configure IIS Web Server

### 1. Aim
To install and configure IIS Web Server.

### 2. Requirement Table
- Windows Server
- IIS Role
- Web Browser

### 3. Procedure

**Step 1:** Open Server Manager → Manage → Add Roles and Features

**Step 2:** Select Web Server (IIS)

**Step 3:** Click Next → Install

**Step 4:** After installation, open browser

**Step 5:** Type: `http://localhost`

**Step 6:** Default IIS page appears

### 4. Theory

IIS (Internet Information Services) is a web server developed by Microsoft for hosting websites, web applications, and web services on Windows Server. It supports multiple protocols including HTTP, HTTPS, FTP, and SMTP.

IIS provides a platform for deploying ASP.NET applications, PHP websites, and static HTML content. Its modular architecture allows administrators to install only needed components, reducing security risks and improving performance. Application pools provide process isolation, preventing one application from affecting others.

IIS integrates with Windows security features including authentication, SSL/TLS encryption, and IP restrictions. It supports URL rewriting, compression, and caching for performance optimization. Management can be done through IIS Manager GUI, command-line tools, or PowerShell scripts.

### 5. Output

Default IIS web page displayed successfully.

### 6. Conclusion

IIS web server was installed and tested successfully.
