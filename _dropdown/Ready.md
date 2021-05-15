---
layout: page
title: Ready
description: Write-up
dropdown: HTB-BOXES
priority: 3
---
# Ready Walkthrough
![](https://ibb.co/MNvrF0n)



# []()Methodology

* Nmap scan.
* Web enumertation.
* Jackson RCE cve.
* Getting reverse shell.
* Enumerate Time.
* Getting reverse shell.
* RooTed.

## Nmap Scan:

**I just found 22 for ssh and 5080 for nginx.**


![](https://i.ibb.co/yVnQLPW/image.png)

### Check the Nginx service:

![](https://i.ibb.co/6s9FbLF/image.png)

> * It's a Gitlab Service run on Nginx.

> * I registered an account y4h1a:y123456789 

> * It's Version 11.4.7 so i will try to find a CVE for this version.

![](https://i.ibb.co/fFgVMw0/image.png)

> **I found an exploit to get RCE via SSRF vulnerability.**

> * You can do it Manually using this [Method](https://github.com/jas502n/gitlab-SSRF-redis-RCE).

> * I did it with this [exploit](https://github.com/ctrlsam/GitLab-11.4.7-RCE).

* by reading the exploit..

```python
username = args.u
password = args.p
gitlab_url = args.g + ":5080"
local_ip = args.l
local_port = args.P

------------
# Create project
import_url = "git%3A%2F%2F%5B0%3A0%3A0%3A0%3A0%3Affff%3A127.0.0.1%5D%3A6379%2Ftest%2F.git"
project_name = f'project{random.randrange(1, 10000)}'
project_url = f'{gitlab_url}/{username}'

```

> **We will bypass ssrf protection using this payload `[0:0:0:0:0:ffff:127.0.0.1]` and get the reverse shell in our listner. this is the exploit will do.**




![](https://i.ibb.co/4fwTjjV/image.png)


![](https://i.ibb.co/V9RW1Y7/image.png)

### Got the User flag!

![](https://i.ibb.co/NjrQ57w/image.png)



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
