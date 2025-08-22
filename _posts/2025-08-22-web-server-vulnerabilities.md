---
title: "Exploiting Web Server Vulnerabilities"
date: 2025-08-22
categories: [Labs, Web Security]
tags: [web-vulnerabilities, directory-traversal, misconfiguration]
---

## Introduction
In this lab, I explored how insecure web server configurations and vulnerabilities can be exploited by attackers to gain unauthorized access to files and directories.

---

## Enumeration
I started by scanning the web server with:
```bash
gobuster dir -u http://<target-ip>/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

This revealed hidden directories such as `/admin/` and `/backup/`.

---

## Exploitation
By navigating to the discovered `/backup/` directory, I found unprotected `.zip` archives containing application source code and database credentials.  

Additionally, the `/admin/` directory allowed direct access to an admin panel with weak authentication.  
This provided an entry point for privilege escalation.

---

## Mitigation
To secure web servers:
- Disable directory listing and restrict access to sensitive directories.
- Avoid storing backup files in web-accessible locations.
- Implement strong authentication for admin panels.
- Regularly patch and update web server software.

---

## Conclusion
This lab highlighted how attackers can exploit insecure web server configurations to access sensitive files and admin interfaces.  
Proper configuration and patching are critical to reducing attack surfaces.
