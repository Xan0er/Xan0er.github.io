---
layout: post
title: "Engineer CTF 2022 Writeups"
date: 2022-03-06 17:15:00 +0530
categories: Infosec
tags: ctf engineer-ctf 2022
---

Hello everyone, welcome to back to another one of my writeup. It's been a while since I have added a writeup for the CTFs. Last weekend, I participated in **Engineer CTF 2022**. I couldn't played many challenges since I have been rusted because I'm playing after a long time. Therefore, I ended up at **134th** position with **960** points. Enjoy the writeup.

<br>
<img src="/assets/images/engineer_ctf_2022/score.png" alt="score">

<br>
<h1><b><u>OSINT</u></b></h1>

<br>
## Vacation

<br><img src="/assets/images/engineer_ctf_2022/vacation_1.png" alt="vacation">

In this challenge, we are given an handle with name of **@tig3r_and_b3ar**. Apparently, this handle belongs to one of the social media accounts on the internet. So firstly, I looked through the Instagram and I found this account on my first try. Yaaaaaayyyy!

<br><img src="/assets/images/engineer_ctf_2022/vacation_2.png" alt="vacation">

As you can see in the above image, this account only had one image of a dog. When I checked the **following** section of this account, I found the account of the owner of this account.

<br><img src="/assets/images/engineer_ctf_2022/vacation_3.png" alt="vacation">

The owner of this account had the **email address** listed in her bio and it said to contact her. So, the next thing which came to my mind is that I should write an email to the given email address.

<br><img src="/assets/images/engineer_ctf_2022/vacation_4.png" alt="vacation">

So, I wrote the above email asking her questions like **What is the flag**. And to my surprise, in respond, I got the below text which contained the flag for this challenge.

<br><img src="/assets/images/engineer_ctf_2022/vacation_5.png" alt="vacation">

Hurray! The flag for this challenge is:
> CTF{f0und_th3_fl@g}

<br>
<h1><b><u>Web</u></b></h1>

<br>
## You're too blind to see

<br><img src="/assets/images/engineer_ctf_2022/too_blind_1.png" alt="too_blind">

Here is **WEB** Category challenge in which we'll have to find the flag by any means. So when we open up the website, it presents us with a login form as you can see below:

<br><img src="/assets/images/engineer_ctf_2022/too_blind_2.png" alt="too_blind">

Let's take look at the source code of the webpage. In the source code of the webpage, we found an encoded text in the form of **Base64**.

<br><img src="/assets/images/engineer_ctf_2022/too_blind_3.png" alt="too_blind">

After decoding the **Base64** text, we found the following piece of text:

<br><img src="/assets/images/engineer_ctf_2022/too_blind_4.png" alt="too_blind">

As per the above text, the person with the email address knows the password. So, I thought that maybe like in the OSINT challenge, I'll have to write an email to the person in order to get the password so I did the same.

<br><img src="/assets/images/engineer_ctf_2022/too_blind_5.png" alt="too_blind">

After writing the above email asking for the password, I got the following response:

<br><img src="/assets/images/engineer_ctf_2022/too_blind_6.png" alt="too_blind">

Now that we know that the password is ***"never_gonna_say_goodbye"***, let's login to the website with **Rick Astley** as the username.

<br><img src="/assets/images/engineer_ctf_2022/too_blind_7.png" alt="too_blind">

And voiila! We have the flag for this challenge:
> CTF{W3r3_N0_57r4N63r5_70_10V3}

<br>
<h1><b><u>Forensics</u></b></h1>

<br>
## The Cat Knows The Culprit

<br><img src="/assets/images/engineer_ctf_2022/cat_culprit_1.png" alt="cat_culprit">

We are given an image file with format **.jfif** (JPEG File Interchange Format) in this challenge. So, our task is to find the hidden flag in the given image.

<br><img src="/assets/images/engineer_ctf_2022/cat_culprit_2.png" alt="cat_culprit">

Whenever I get an image file in a CTF, the first thing I do is to check the file details with the **Exiftool**.

<br><img src="/assets/images/engineer_ctf_2022/cat_culprit_3.png" alt="cat_culprit">

As you can see in the above image, along with the other details of the image file, we also got our flag. It is written down under the **comment** section.

So, here is the flag for this challenge:
> CTF{It_1s_D0UG_JUdY}

<br>
## Save Our Souls!

<br><img src="/assets/images/engineer_ctf_2022/souls_1.png" alt="souls">

As per the challenge description, we have to find the hidden message inside the message audio file. So, let's open up the **Sonic Visualiser** and analyze our audio file. When we tried to open the file, it gives us the following error which can only mean that the header of the file is not properly set and we'll have to fix that first.

<br><img src="/assets/images/engineer_ctf_2022/souls_2.png" alt="souls">

When we ran the **file** command on the **message.wav** file, it gave us the following output which means that this file is not a **wav** file.

<br><img src="/assets/images/engineer_ctf_2022/souls_3.png" alt="souls">

Now, let's open up this file with the command-line utility called **hexedit** and fix this file by replacing the first four bytes of the file 
```
23 69 52 96
```
with
```
52 49 46 46
```

<br><img src="/assets/images/engineer_ctf_2022/souls_4.png" alt="souls">

After fixing up the file, let's run the **file** command again and see what output we get this time.

<br><img src="/assets/images/engineer_ctf_2022/souls_5.png" alt="souls">

As you can see in the above image, our file has been fixed. Now, let's open it in the **Sonic Visualiser** and play it. Upon playing the file, I didn't found anything interesting so let' try another approach. I added an layer of spectogram and let's see what we get this time.

<br><img src="/assets/images/engineer_ctf_2022/souls_6.png" alt="souls">

Yay! We found our flag:
> CTF{WE-NEED-REINFORCEMENTS}

<br>
**I hope you enjoyed reading this writeup. If you enjoyed this, I keep posting writeups on this blog so stay tuned for more.**