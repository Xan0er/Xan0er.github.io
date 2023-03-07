---
layout: post
title: "San Diego CTF 2022 Writeups"
date: 2022-05-08 17:15:00 +0530
categories: CTF
tags: sandiego_ctf
---

This week I particpated in the **San Diego CTF 2022** and it was a blast. I wasn't able to solve many challenges but I did manage to solve some of the challenges. This is the first CTF that I played on a discord server. I had a lot of fun while playing this CTF. Hope you have as much fun while reading writeups as I had when I played this CTF.

Also, I have completed the writeup of this CTF after almost a year since I was busy so there will be some challenges which I won't be able to explain properly since those challenges URLs have been taken down by the organizers such as the Web based challenges.

<br>
<h1><b><u>OSINT</u></b></h1>

<br>
## The Flag Has Been Stolen

<br><img src="/assets/images/san_diego_ctf_2022/stolen_flag_1.png" alt="stolen_flag">

<br>
For this challenge, we have to find the flag on the SD CTF website by visiting the wayback machine since the flag was in the past but it has not been deleted. I got this clue by viewing the video tutorial mentioned in the challenge description.

<br>
The wayback machine doen't have a long history so it should be fiarly easy to find the flag.

<br><img src="/assets/images/san_diego_ctf_2022/stolen_flag_2.png" alt="stolen_flag">

<br>
Found the flag as easy as it is:
> sdctf{Th3_L0$t_trE@$ur3_0f_th3_INt3rnEt}

<br>
## Google Ransom

<br><img src="/assets/images/san_diego_ctf_2022/google_ransom_1.png" alt="google_ransom">

<br>
In the ransom letter, we see a cryptic note about someone threatening to sign their email up for an annoying newsletter. As threatening as this is, it doesn’t give us any information about who made this google doc. However, since it’s google there is likely a way to view the email of this account owner. 
I went to the **Shared With Me** section on google drive to get more information about the random letter. 
Looking at Details, we can see that the owner of the document is **Amy SDCTF**.

<br>
After sending `amy.sdctf@gmail.com` an email demanding the flag, we get a response giving it to us.

<br><img src="/assets/images/san_diego_ctf_2022/google_ransom_2.png" alt="google_ransom">

> sdctf{0p3n_S0uRCE_1S_aMaz1NG}

<br>
## Mann Hunt

<br><img src="/assets/images/san_diego_ctf_2022/mann_hunt_1.png" alt="mann_hunt">

<br>
This was a very interesting challenge since we are given only the username of the hacker (**mann5549**) which we have to track down. Our objective is to find the email address of this hacker and send them an email demanding the flag. So, lets' start by searching this username and finding on which websites this username is being used using the [Sherlock Tool](https://github.com/sherlock-project/sherlock). 

<br><img src="/assets/images/san_diego_ctf_2022/mann_hunt_2.png" alt="mann_hunt">

<br>
We got a total of 8 results from the sherlock. I've checked all of the links but only one of them seems useful to me and that is the twitter account.

<br><img src="/assets/images/san_diego_ctf_2022/mann_hunt_3.png" alt="mann_hunt">

<br>
In the bio of the mann twitter account, I found the link to this hacker website. Let's visit this site and see if we can find anything juicy stuff that can put this hacker behind bars. The website is a static site so there's not much interactivity on the webpages.

<br><img src="/assets/images/san_diego_ctf_2022/mann_hunt_4.png" alt="mann_hunt">

<br>
Let's check the source code of the website and see if we can find any comments or better, the email address of the mann. To my surprise, I couldn't find the email address of the mann or any useful comments but I did found the link to the github profile of the mann where he is hosting this website from.

<br><img src="/assets/images/san_diego_ctf_2022/mann_hunt_5.png" alt="mann_hunt">

<br><img src="/assets/images/san_diego_ctf_2022/mann_hunt_6.png" alt="mann_hunt">

<br>
Now, let's see if we can figure out the email address of mann from the one repository in github account. We can see that a couple of things have been deleted, again, due to our target knowing that they are being tracked down.

<br>
There are only 2 commits in the repository and in the latest commit, there are a total of 6 files here that have been changed. I reviewed the changes that the hacker has committed recently but I couldn't find any information related to the email address. I then started to read each of the changed file completely not just the changes. There is one file which is named by gatsby-config.js. In this config file, there is a site **siteMetadata** section and this section has a **description** key which says `Blogger and Coder for ACM at UC San Diego`. 

<br><img src="/assets/images/san_diego_ctf_2022/mann_hunt_7.png" alt="mann_hunt">

<br><img src="/assets/images/san_diego_ctf_2022/mann_hunt_8.png" alt="mann_hunt">

<br><img src="/assets/images/san_diego_ctf_2022/mann_hunt_9.png" alt="mann_hunt">

<br>
I decided to just search the found description on the google search and see if any results related to it comes up and I found a linkedin profile.

<br><img src="/assets/images/san_diego_ctf_2022/mann_hunt_10.png" alt="mann_hunt">

<br>
In the "**About**" section of the hacker LinkedIn page, there is a link to download mann resume. I visit the link to look at the resume and notice that the email I need to retrieve has been replaced with asteriks.

<br><img src="/assets/images/san_diego_ctf_2022/mann_hunt_11.png" alt="mann_hunt">

<br>
Luckily for me, in the right-hand panel under the sharing section I can hover over mann profile and it displays the email address of the hacker we are looking for. Finally! We found the email address.

<br><img src="/assets/images/san_diego_ctf_2022/mann_hunt_12.png" alt="mann_hunt">

<br>
All that's left now is to send an email to this hacker demanding that he give our flag back to us.

<br><img src="/assets/images/san_diego_ctf_2022/mann_hunt_13.png" alt="mann_hunt">

<br><img src="/assets/images/san_diego_ctf_2022/mann_hunt_14.png" alt="mann_hunt">

<br>
So, here's our flag that the hacker stole from us:
> sdctf{MaNN_tH@t_w@s_Ann0YinG}

<br>
<h1><b><u>Crypto</u></b></h1>

<br>
## Vinegar

<br><img src="/assets/images/san_diego_ctf_2022/vinegar_1.png" alt="vinegar">

<br>
In this challenge, we are given a cipher text which we have to decode.
```
{wbeyrjgewcfroggpesremvxgvefyrcmnnymxhdacgnnrwprhxpuyyaupbmskjrxfopr}
```

<br>
The name of the challenge **Vinegar** is also an hint which says that this text could be encoded with the vignere cipher. So, I went to the [Dcode Vigenere Cipher](https://www.dcode.fr/chiffre-vigenere) website to decode this code.

<br>
I got a partial key (**UNKNO**) when we tried to decrypt this cipher with the **Automatic Decryption**. So, now I used this key as decryption method to see if I can get the full decoded text.

<br><img src="/assets/images/san_diego_ctf_2022/vinegar_2.png" alt="vinegar">

<br><img src="/assets/images/san_diego_ctf_2022/vinegar_3.png" alt="vinegar">

<br>
I finally found the key and decrypted text.

<br><img src="/assets/images/san_diego_ctf_2022/vinegar_4.png" alt="vinegar">

<br>
So, the challenge flag would be:
> sdctf{couldntuseleetstringsinthisonesadlybutwemadeitextralongtocompensate}

<br>
<h1><b><u>Forensics</u></b></h1>

<br>
## Flag Trafficker

<br><img src="/assets/images/san_diego_ctf_2022/flag_trafficker_1.png" alt="flag_trafficker">

<br>
The given pcap network packet capture file is what we have to analyze in this challenge in order to capture the flag. I downloaded the file and fired up my wireshark to start analyzing the file.

<br><img src="/assets/images/san_diego_ctf_2022/flag_trafficker_2.png" alt="flag_trafficker">

<br>
Whenever I start analyzing a pcap capture file, I make sure to check if there are any files or objects which I can export. You can find this options by visiting `File > Export Objects`.

<br>
I saw an HTML file while analyzing the HTTP objects list. It could mean that this file may contain our flag but we still have to wait and analyze this file.

<br><img src="/assets/images/san_diego_ctf_2022/flag_trafficker_3.png" alt="flag_trafficker">

<br>
I opened up the HTML file in the **Firefox** browser. There was a one button in the HTML file which says that `Click to display the flag!`. I clicked on this button and luckily this was not a fake flag but this is the actual flag.

<br><img src="/assets/images/san_diego_ctf_2022/flag_trafficker_4.png" alt="flag_trafficker">

<br>
So, this is the flag that was being trafficked:
> sdctf{G3T_F*cK3d_W1r3SHaRK}

<br>
**I really had a lot of file playing this CTF and I thanks the organizers for hosting this CTF on the discord platform since this was a new experience for me. Let's hope that they will host CTF again next year again untill then see you soon!**