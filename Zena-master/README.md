# Zena
Proof of Concept code for XSS to CSRF to Remote Code Execution exploit for asg-zena
CVE-2021-45025 Cleartext Storage of Sensitive Information in a Cookie
CVE-2021-45026 Stored Cross Site Scripting (XSS)
Authors: James Barnett and Jeffrey Green
**To Run:**
- python CookieMonster.py <hostname/ip> <TLS/SSL - True or False> <cmd.exe command>
  - **Example: python3 CookieMonster.py 127.0.0.1 False "/c whoami > c:/out.txt"**


Basic payload and details and injection points

Zena – ClientManager:
XSS(Cross-Site Scripting):
• Type: Stored XSS – Unauthenticated
• Pages Affected: /zena/index.html
• Description: User input field(and probably more fields) on the Zena ClientManager login page allow for
an attacker to submit specially crafted javascript payloads that are persistently stored and executed
when a user on the Zena system navigates to Client Manager logs and shows details for the log.
POC code – placed into the username field on the login page: 
```</li><img/src='fail'/onerror=alert("vulnerable")></img>```

XSS(Cross-Site Scripting):
• Type: Stored XSS
• Pages Affected: /webconfig/index.html requires authentication. the webconfig page has a hardcoded default password.
• Description: Certain input fields in the connector creation allow for an attacker to submit specially
crafted arbitrary javascript payloads that are persistently stored and executed when loading the page
POC code - placed into the Name input field:
```</li><img/src='fail'/onerror=alert("vulnerable")></img>```
