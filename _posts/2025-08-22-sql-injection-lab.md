---
title: "SQL Injection Lab Walkthrough"
date: 2025-08-22
categories: [Labs, Web Security]
tags: [sql-injection, authentication-bypass, union-select, database]
---

## Introduction
In this lab, I explored how SQL injection vulnerabilities can be abused to bypass authentication and extract sensitive data from a backend database. SQLi persists as a top OWASP risk due to unsanitized inputs and unsafe query construction.

---

## Environment & Scope
- **Target:** Vulnerable login form backed by a SQL database
- **Goal:** Bypass login and enumerate data
- **Tools:** Browser, proxy (optional), and basic SQL payloads

---

## Enumeration
I tested the login form with simple special characters to observe error messages and behavior changes.

**Indicator of SQLi:** entering a single quote `'` in the username field produced anomalous behavior, suggesting the input was being concatenated directly into a SQL statement.

---

## Exploitation
### Step 1 — Authentication Bypass
A classic boolean-based payload allowed login bypass:

```
' OR '1'='1' --
```

- The trailing `--` comments out the rest of the original query.
- The `OR '1'='1'` condition evaluates to true, forcing the query to return a valid row.

### Step 2 — Data Enumeration with `UNION SELECT`
After confirming SQLi, I leveraged `UNION SELECT` to enumerate and extract data:

```sql
' UNION SELECT username, password FROM users --
```

> Notes:
> - Column counts must match the original query.
> - If unknown, determine the number of columns via `ORDER BY` testing or incremental `UNION SELECT NULL,...`.

---

## Evidence (Example)
- Successful login without valid credentials
- Retrieved sample rows from `users` table via `UNION SELECT`

---

## Mitigation
- **Use parameterized queries / prepared statements** (bind variables rather than string concatenation).
- **Server-side input validation & output encoding** (deny-list risky patterns; prefer allow-lists).
- **Least-privilege DB accounts** (restrict application user to only required statements/tables).
- **Error handling** (don’t leak stack traces or SQL errors to users).
- **WAF / RASP** as compensating controls; monitor anomalies and block known payload patterns.
- **Secure SDLC & testing** (static/dynamic testing and regular dependency patching).

---

## Conclusion
This lab reinforced how trivial inputs can subvert insecure SQL queries, enabling authentication bypass and data exfiltration. Robust parameterization, validation, and least privilege are essential to prevent SQL injection.
