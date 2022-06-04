# Zena
## Proof of Concept code for XSS to CSRF to Remote Code Execution exploit for asg-zena

CVE-xxxx-xxxxx Cleartext Storage of Sensitive Information in a Cookie

CVE-xxxx-xxxxx Stored Cross Site Scripting (XSS)

Authors: James Barnett and Jeffrey Green
**To Run:**
- python CookieMonster.py <hostname/ip> <TLS/SSL - True or False> <cmd.exe command>
  - **Example: python3 CookieMonster.py 127.0.0.1 False "/c whoami > c:/out.txt"**

