---
title: "SQL Injection Lab Walkthrough (Capstone)"
date: 2025-08-24
categories: [Labs, Web Security]
tags: [sql-injection, authentication-bypass, union-select, database, password-cracking]
excerpt: "In this capstone lab, I exploited a SQL injection vulnerability to enumerate users, crack a password hash, and retrieve a hidden flag file."
---

## Objective
The goal of this lab was to exploit a SQL injection vulnerability on a target web application, retrieve user account information, crack a password hash, and use the compromised credentials to access a hidden flag file.

---

## Environment & Scope
- **Target IP:** `10.5.5.12` (DVWA web app)  
- **Secondary target:** `192.168.0.10` (Bob Smith’s host)  
- **Tools used:** Browser (DVWA), `nmap`, `ftp`, CrackStation  
- **Focus:** Exploiting SQL injection and lateral movement

---

## Step 1 — Authentication Bypass & SQL Injection
Testing the login form with a classic injection payload:

```sql
' OR '1' = '1' #
```

This bypassed authentication and confirmed SQL injection.  

![SQLi login bypass](/assets/images/labs/sql-injection/SQLi-1.png)

---

## Step 2 — Extracting User Data
To enumerate user accounts and password hashes:

```sql
' UNION SELECT user, password FROM users #
```

This revealed multiple user accounts, including **Bob Smith** with a password hash.  

![Union select results](/assets/images/labs/sql-injection/SQL-passwordhashes.png)

---

## Step 3 — Cracking the Password Hash
I copied Bob’s hash and cracked it using [CrackStation](https://crackstation.net/).  

- **Recovered password:** `password`  

![Crackstation results](/assets/images/labs/sql-injection/crackedhash.png)

---

## Step 4 — Host Enumeration
Next, I targeted Bob Smith’s machine (`192.168.0.10`) and scanned for open services:

```bash
nmap -sS -sV -p- 192.168.0.10
```

This revealed an **FTP service running on port 2121**.  

![Nmap scan results](/assets/images/labs/sql-injection/nmapfullscan.png)

---

## Step 5 — Accessing the FTP Server
Using Bob’s credentials, I logged in via FTP:

```bash
ftp 192.168.0.10 2121
```

I located a file named `my_passwords.txt` in his home directory.  

![FTP session](/assets/images/labs/sql-injection/ftploginport2121.png)

---

## Step 6 — Retrieving and Reading the Flag
Downloaded the file:

```bash
get my_passwords.txt
```

Then displayed its contents:

```bash
cat my_passwords.txt
```

The file contained the **Challenge  code**.  

![Flag file contents](/assets/images/labs/sql-injection/flagfile.png)

---

## Mitigation
To protect against SQL injection and credential compromise:
- **Parameterized queries** (prepared statements with bound variables).  
- **Input validation & sanitization** (reject unsafe characters, enforce whitelists).  
- **Error handling** (suppress SQL errors from users).  
- **Strong password storage** (salted & hashed with modern algorithms).  
- **Least privilege** (restrict database and file permissions).  

---

## Conclusion
This lab demonstrated how SQL injection can be escalated to full credential compromise and system access. Proper coding practices, secure password handling, and strict access controls are essential to prevent such attacks.
