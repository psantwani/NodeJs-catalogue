1. ```setInterval```, ```setTimeout``` and ```new function()``` also use eval. Vulnerable to injection attacks and is slow (runs compiler)
2. ```use strict``` to avoid silent errors
3. Use ```child_process.execFile``` instead of ```child_process.exec``` as the later makes a call to execute ```/bin/sh``` (a bash interpreter, not a program launcher)
4. Never insert untrusted data into DOM and use HTML escape before inserting to avoid Reflected XSS (attacker injects executable code in HTTP response)
5. Set ```HttpOnly``` flag on cookies to prevent cookie theft. e.g. ```var cookies = document.cookie.split('; ');```
6. Set HTTP header ```Content-Security-Policy: default-src 'self' *.mydomain.com``` to mitigate XSS type attacks. It allows content from a trusted domain/subdomain only

# Tools
1. ```helmet``` - Implements middlewares like csp, crossdomain, xframe, xssfilter, etc.
2. ```npm shrinkwrap``` - Locks down dependency versions recursively
3. ```retire.js``` - Detects module versions with vulnerabilties
4. [ratelimiter](https://www.npmjs.com/package/ratelimiter) package against brute-force attack.
5. [cookie-session](https://www.npmjs.com/package/cookie-session) for secure session management.
6. [csurf](https://www.npmjs.com/package/csurf) module against cross-site request forgery (forcing a logged in user to execute unwanted actions on a webapp). 
7. [sqlmap](http://sqlmap.org/), a penetration testing tool detecting and exploiting SQL injection vulnerabilities in your code. 
8. [nmap](https://nmap.org/) for SSL version, algorithm and key length.
Checking certificate information
```
nmap --script ssl-cert,ssl-enum-ciphers -p 443,465,993,995 www.example.com
```
9. Testing SSL vulnerabilities with [sslyze](https://github.com/iSECPartners/sslyze)
```
./sslyze.py --regular example.com:443
```

# Security HTTP Headers
1. ```Strict-Transport-Security```: enforces secure(HTTP over SSL) connections to the server
2. ```X-XSS-Protection```: Enables XSS filter into most recent browsers
3. ```X-Content-Type-Option```: prevents browser from MIME-snipping a response away from the declared content-type
4. ```Content-Security-Policy```: Prevents XSS attacks.
You can set these in your nginx/apache config too.

# HSTS
Strict transport security header enforcing HTTPS connections to server. ```strict-transport-security:max-age=631138519```. ```max-age``` defines the number of seconds that the browser should automatically convert all HTTP requests to HTTP
Testing it: ```curl -s -D- https://twitter.com/ | grep -i Strict```

# DOS (Denial of Service)
1. Account lockout - Some number of unsuccessful login attempts lead to system prohibiting the IP address/account for a given period.
2. Evil regexes - Exploiting Regex implementations such that it becomes a very heavy operation (ReDOS - Regex DOS). Use [safe-regex](https://www.npmjs.com/package/safe-regex)

# Error Handling
1. Error codes, stack traces - Dont leak out sensitive info to the users like ```X-Powered-By:Express```

# NPM
1. use ```nsp``` module or [requireSafe](https://www.npmjs.com/package/requiresafe) to check vulenrabilties in the NPM modules used in your project.
2. [Snyk](https://snyk.io/) not only will detect vulnerabilities but will also fix them.