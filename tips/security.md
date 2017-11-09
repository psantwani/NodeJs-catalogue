1. ```setInterval```, ```setTimeout``` and ```new function()``` also use eval. Vulnerable to injection attacks and is slow (runs compiler)
2. ```use strict``` to avoid silent errors
3. Use ```child_process.execFile``` instead of ```child_process.exec``` as the later makes a call to execute ```/bin/sh``` (a bash interpreter, not a program launcher)
4. Never insert untrusted data into DOM and use HTML escape before inserting to avoid Reflected XSS (attacker injects executable code in HTTP response)
5. Set ```HttpOnly``` flag on cookies to prevent cookie theft. e.g. ```var cookies = document.cookie.split('; ');```
6. Set HTTP header ```Content-Security-Policy: default-src 'self' *.mydomain.com``` to mitigate XSS type attacks. It allows content from a trusted domain/subdomain only

# Tools
1. ```helmet``` - Implements middlewares like csp, crossdomain, xframe, xssfilter, etc.
2. ```npm shrinkwrap``` - Locks down dependency versions recursively
3. ````retire.js``` - Detects module versions with vulnerabilties