---
layout: page
title: Launne
description: Write-up
dropdown: HTB-BOXES
priority: 1
---
# Launne Walkthrough
![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/68dcaeda-cf61-48b1-81df-3b50af1e1e63/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T212031Z&X-Amz-Expires=86400&X-Amz-Signature=dbcd74a0e70df071916f3b5829e6908e7810a2b0a601ab5e8b73336fbfb8017b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)



# []()Methodology

* Nmap scan.
* Web enumertation.
* Lua web application security vulnerabilities.
* Getting reverse shell.
* Enumerate launne.
* Getting id_rsa for a user.
* ssh connecting.
* Privilege escalation.
* RooTed.

## Nmap Scan:

**I Found 3 ports opened 22,80,9001.** 

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/64c7e259-9168-4229-9667-2595a8ddbe3a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T195051Z&X-Amz-Expires=86400&X-Amz-Signature=6e899b602e9231418485f298e07fb90c8d05390be5262e8817c037c988b418cd&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)


### Let's start with the Web Page:

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4d331ad0-6ca7-4f1d-909f-84f926e8d56e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T220747Z&X-Amz-Expires=86400&X-Amz-Signature=65b805dc8bf10b8ecc4d26e664344b6d9bee398bcc14798209258e3103428039&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

> **It has authentication pop-up.**

> **I Tried admin:admin and other random credentials but no thing fine to login.**

> **But i noticed that when i got (401 Unauthorized) that is a localhost web page at port 3000.**

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3a02ba02-588c-4963-9dc7-24ea4943787e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T220955Z&X-Amz-Expires=86400&X-Amz-Signature=cc099e62053f11be44e021d0cb5e4789522f0935ca106604f5f0521d40de01a3&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)


> #### I used ffuf for fuzzing launne.htb/


![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f6b5b7fb-096e-4093-8e4e-c60e9995e58e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T221725Z&X-Amz-Expires=86400&X-Amz-Signature=8fc77eddae8de86bd36a0cf5c68b9a9fabfd4662dd30c88f17b2a0a8bed8094f&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)



* We got robots.txt

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6ab319aa-1ec4-4590-b79a-3a1c662e4592/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T221840Z&X-Amz-Expires=86400&X-Amz-Signature=d5b1629ee14b8a6800c3adbee52baa8619ee207b69bb3ef4a574e9163fb2d6b0&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

* /weather disallowd with comment: still harversting cities.

* access to launne.htb/weather 404 not found.

* but there are cities we had to find them.

* let's fire up ffuf again to fuzz launne.htb/weather/ 

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4cc16698-a933-4e1f-b8fe-93d90e352b09/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T222958Z&X-Amz-Expires=86400&X-Amz-Signature=671502ab015208afeb98b1e8115c50c54a76c79eac2badc5047ca95c6a2786ca&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

**Nice got forecast**

> * by visiting launne.htb/weather/forecast/ got this.


![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/30d8f697-acbb-4e76-a9d3-f180eef28422/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T223205Z&X-Amz-Expires=86400&X-Amz-Signature=54f272392356033508eb550b63db92bf354720ed6607b1ca1a05839ebf2b4a08&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)


> #### Use city=list to list available cities 

*okay, Thanks*


![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/97f1a229-81a0-4fc5-a5bd-a1563ef5d03a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T223414Z&X-Amz-Expires=86400&X-Amz-Signature=1a4dc9c7f0f8694c1e8fa955301877b599fc83c7420080adeb386ce8f37a0d03&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)



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
