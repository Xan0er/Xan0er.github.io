---
layout: post
title: "DeadFace CTF 2022 Writeups"
date: 2022-10-17 20:15:00 +0530
categories: CTF
tags: ctf deadface-ctf 2022
---

Welcome back to another writeup of the **DeadFace CTF**. This is probably one of the my favourite CTFs that I have been playing since 2021. There is a nicely build storyline related to a group of hackers called **DeadFace** and we'll have to stop them from breaking into systems or gathering information on the members of this group. I captured flags with **380** points of worth which landed me at the **333rd** position. So, without any further delay, let's the learning begins!

<br><img src="/assets/images/deadface_ctf_2022/score.png" alt="score">

<br>
<h1><b><u>OSINT</u></b></h1>

<br>
## Under Public Scrutiny

<br><img src="/assets/images/deadface_ctf_2022/under_public_scrutiny_1.png" alt="under_public_scrutiny">

<br>
As per the challenge description, there is a forum called **Ghost Town** in which all the members of the **DeadFace** communicate with each other and expresses their opinions. So, we have to explore that forum in order to find the Github repository of this hacker group. I found a thread on the forum with title `Made a github link for projects`.

<br><img src="/assets/images/deadface_ctf_2022/under_public_scrutiny_2.png" alt="under_public_scrutiny">

<br>
The github account username is `deadf4c3` as mentioned in the reply sent by a user `bumpyhassan`.

<br><img src="/assets/images/deadface_ctf_2022/under_public_scrutiny_3.png" alt="under_public_scrutiny">

<br>
When you visit the github account of this user, there is one repository in this user account with title `tarrasque`. So, I opened up this repository and we got our flag in the `README.md` file of the repository.

<br><img src="/assets/images/deadface_ctf_2022/under_public_scrutiny_4.png" alt="under_public_scrutiny">

> flag{yAy_4_puBl1c_g1tHUB_rep0s}

<br>
## Fine Dining

<br><img src="/assets/images/deadface_ctf_2022/fine_dining_1.png" alt="fine_dining">

<br>
Now, our challenge is to track down a member whose father owns a restaurant. So, if we can figure out where the restaurant is, then we can narrow down the area where our target is living. Once again, we have to go the forum called **Ghost Town** in order to find the clues about the restaurant. I find a thread titled `Suggestions for places to eat`. Let's explore this thread and see if we can find any clue.

<br><img src="/assets/images/deadface_ctf_2022/fine_dining_2.png" alt="fine_dining">

<br>
I came across a member with alias as `mirveal` and this member has mentioned that their dad owned a restaurant but it was shut down last year. Also, he/she mentioned that the restaurant was in the **CA (California, USA)**. The name of the restaurant was **Pig & Tall**. It mention of his/her dad's restaurant also appeared in the newspaper so he's gonna see if he/she can find the newspaper clippings. 

<br><img src="/assets/images/deadface_ctf_2022/fine_dining_3.png" alt="fine_dining">

<br>
Then, **mirveal** sent a link in the thread with the newspaper clipping from the 90s.

<br><img src="/assets/images/deadface_ctf_2022/fine_dining_4.png" alt="fine_dining">

<br>
In the newspaper clipping, we can see that the restaurant is opened by **Kaku Shiwaku** which is probably the members's dad name and then the street name is also mentioned on which the restaurant is opened which is **E Pine Street**. In the same clipping, flag is also there for this challenge.

<br><img src="/assets/images/deadface_ctf_2022/fine_dining_5.jpg" alt="fine_dining">

> flag{b1sh0p-pig-n_T4LL}

<br>
<h1><b><u>Forensics</u></b></h1>

<br>
## First Strike

<br><img src="/assets/images/deadface_ctf_2022/first_strike_1.png" alt="first_strike">

<br>
For this challenge, we are given two log files `access.log` & `error.log`. I loaded these files in my [Splunk Cloud](https://www.splunk.com/) instance so that we can easily analyze the logs.

<br><img src="/assets/images/deadface_ctf_2022/first_strike_2.png" alt="first_strike">

<br>
So, first of all, let's checkout the client IPs which made requests to the host. 

<br><img src="/assets/images/deadface_ctf_2022/first_strike_3.png" alt="first_strike">

We can see that there is only one IP address in the `access.log`. And that IP address is **165.227.73.138**. A total of `7,429` requests originated from this IP address so this is probably the IP address from where the attack originated.

> flag{165.227.73.138}

<br>
## Toolbox

<br><img src="/assets/images/deadface_ctf_2022/toolbox_1.png" alt="toolbox">

<br>
Now, our objective is to find the tool use when the attack started at **2022-07-27 14:13 UTC**. So, I added filter to my splunk `access.log` report `date_minute="13"`.

<br><img src="/assets/images/deadface_ctf_2022/toolbox_2.png" alt="toolbox">

<br>
As we can see in the above screenshot, that the tool used when the attack started was `Nmap` which is probably used to scan open ports and services listed on the server.

> flag{nmap}

<br>
## Agent Of Chaos

<br><img src="/assets/images/deadface_ctf_2022/agents_of_chaos_1.png" alt="agents_of_chaos">

<br>
So, what could be the first user agent of the second scanning tool used by the attacker? For this, we'll go back to my `access.log` report to figure out the what was the second scanning tool used. When we see the useragents in the **Splunk** report, we see that the name of the second scanning tool is **Nikto**.

<br><img src="/assets/images/deadface_ctf_2022/agents_of_chaos_2.png" alt="agents_of_chaos">

<br>
The user agent of the **Nikto** scanning tool is `Mozilla/5.00 (Nikto/2.1.6) (Evasions:None) (Test:Port Check)`

> flag{Mozilla/5.00 (Nikto/2.1.6) (Evasions:None) (Test:Port Check)}

<br>
<h1><b><u>SQL</u></b></h1>

<br>
## Counting Heads

<br><img src="/assets/images/deadface_ctf_2022/counting_heads_1.png" alt="counting_heads">

<br>
So, they need my help to figure out the scope of breach, Lol. Are they really hackers or just a bunch of noobies? Anyway, I have decided to help them sort this issue out.

<br>
I loaded the database file in **MySQL** database. My first task is to figure out the total number of users in the database. The tables in the database are as follows:

<br><img src="/assets/images/deadface_ctf_2022/counting_heads_2.png" alt="counting_heads">

<br>
So, the MySQL command to find the total number of users would be:
```
SELECT count(*) FROM users;
```

<br><img src="/assets/images/deadface_ctf_2022/counting_heads_3.png" alt="counting_heads">

So, the number of users in the database are **2400**.

> flag{2400}

<br>
## The Faculty

<br><img src="/assets/images/deadface_ctf_2022/the_faculty_1.png" alt="the_faculty">

<br>
Now, we need to find the total number of compromised users which are not students., So, the MySQL command for such type of condition as per the database and tables would be:
```
SELECT COUNT(DISTINCT US.user_id) 
FROM users US LEFT JOIN roles_assigned RA ON US.user_id=RA.user_id 
WHERE RA.role_id!=1;
```

<br>
In the above MySQL command, I have joined two tables `users` & `roles_assigned` on the common column **user_id**. Then, there is a **WHERE** condition which returns only those records where the **role_id** is not equals to 1 since that is the role ID that is assigned for the users.

<br><img src="/assets/images/deadface_ctf_2022/the_faculty_2.png" alt="the_faculty">

> flag{627}

<br>
## Let's Hash It Out

<br><img src="/assets/images/deadface_ctf_2022/lets_hash_it_out_1.png" alt="lets_hash_it_out">

<br>
In this scenario, we have to find the hash of the password of a target that the group members are talking about on the **Ghost Town** forum. So, our first task is to find the thread on the forum.

<br><img src="/assets/images/deadface_ctf_2022/lets_hash_it_out_2.png" alt="lets_hash_it_out">

<br>
I found a thread on the forum titled `Database question`. In this thread, the group members are discussing about which users they should target from the faculty. Then, someone says that we should target the HR, Administration, etc. first before we target professors. Then, `mirveal` says that we should target the **Administration** first since there is only one user there.

<br><img src="/assets/images/deadface_ctf_2022/lets_hash_it_out_3.png" alt="lets_hash_it_out">

<br>
Now, our first task is to find the **role_id** of the **Admininstration** role. For that, we'll have to go back to our database. The MySQL command for this purpose would be:
```
SELECT * FROM roles;
```

<br><img src="/assets/images/deadface_ctf_2022/lets_hash_it_out_4.png" alt="lets_hash_it_out">

<br>
As we can see in the above screenshot, the **role_id** of the **Administration** role is 8. Now, we need to write the command to fetch the password hash for this role. We will join three tables together: `users`, `roles_assigned`, & `passwords` and then we will add a **WHERE** condition to filter where the role_id is equals to 8.
```
SELECT PA.password 
FROM users US LEFT JOIN roles_assigned RA ON US.user_id=RA.user_id LEFT JOIN passwords PA ON US.user_id=PA.user_id 
WHERE RA.role_id=8;
```

<br><img src="/assets/images/deadface_ctf_2022/lets_hash_it_out_5.png" alt="lets_hash_it_out">

> flag{b487af41779cffb9572b982e1a0bf83f0eafbe05}

<br>
## Fall Classes

<br><img src="/assets/images/deadface_ctf_2022/fall_classes_1.png" alt="fall_classes">

<br>
In this challenge, we need to figure out the count of the fall courses in the database dump. So, first of all, we need to find the term name of the fall classes and the MySQL command for this would be:
```
SELECT * FROM terms;
```

<br><img src="/assets/images/deadface_ctf_2022/fall_classes_2.png" alt="fall_classes">

<br>
For this, we will join two tables: `term_courses` & `terms` in order to find the unique number of courses. Then, we add a **WHERE** condition in which we will filter records where term_name is equals to **FALL2022**. So, the MySQL command would be:
```
SELECT COUNT(DISTINCT TC.course_id) 
FROM term_courses TC LEFT JOIN terms TE ON TC.term_id=TE.term_id 
WHERE TE.term_name="FALL2022";
```

<br><img src="/assets/images/deadface_ctf_2022/fall_classes_3.png" alt="fall_classes">

> flag{405}

<br>
<h1><b><u>Cryptography</u></b></h1>

<br>
## "D" is for Dumb Mistakes

<br><img src="/assets/images/deadface_ctf_2022/d_is_for_dumb_mistakes_1.png" alt="d_is_for_dumb_mistakes">

<br>
This challenge is a basic **RSA Alorithm** (Rivest, Shamir, Adleman Algorithm) challenge in which we are given a few variables used while encrypting the text message.
```
- Prime Number (p) is 1049
- Prime Number (q) is 2063 
- Exponent (e) is 777887  
```

<br>
Now, we need to recompute the decryption key (d) by using the above values. So, in order to find the decryption key, we first need to find the **phi** value first by using the **p** & **q** prime numbers. Then, we will find the inverse value of the **e** & **phi** value in order to compute the decryption key. I have create a simple python script to compute the decryption key:
```
from Crypto.Util.number import inverse # Import python crypto library 

p = 1049
q = 2063
e = 777887

phi = (p-1) * (q-1) # Computing phi value
d = inverse(e, phi) # Computing decryption key

print("Decryption Key:", d)
```

<br>
After running the above script, we will get the decryption key.
> flag{d=1457215}

<br>
## "D" is for Decryption

<br><img src="/assets/images/deadface_ctf_2022/d_is_for_decryption_1.png" alt="d_is_for_decryption">

<br>
This challenge is in continuity with the previous challenge. In the previous challenge, we have re-computed the decryption key value, so by using that decryption key value (**1457215**) we need to decrypt the DEADFACE's private message:
```
992478-1726930-1622358-1635603-1385290
```

<br>
In order to decrypt the above message, I have written a small python script:
```
from string import ascii_lowercase

print("============ Finding Flag ============\n")

# Creating a dictionay of letters as per the indexes in alphabets
ALPHABETS = {index: letter for index, letter in enumerate(ascii_lowercase, start=1)} 

p = 1049
q = 2063
d = 1457215

n = p * q

enc_msg = "992478-1726930-1622358-1635603-1385290"
enc_msg_l = enc_msg.split("-")
flag_parts = []

# Decrypting each part of the flag
for msg_part in enc_msg_l:
	char_pos = pow(int(msg_part), d) % n
	flag_parts.append(ALPHABETS.get(char_pos))

flag = "".join(flag_parts)
print("Flag:", flag)
```

<br><img src="/assets/images/deadface_ctf_2022/d_is_for_decryption_2.png" alt="d_is_for_decryption">

> flag{ghost}

<br>
## Two Dead Boys

<br><img src="/assets/images/deadface_ctf_2022/two_dead_boys_1.png" alt="two_dead_boys">

<br><img src="/assets/images/deadface_ctf_2022/two_dead_boys_2.png" alt="two_dead_boys">

<br>
This is a tale about the two boys and how they fight the dead, I think. There is some encrypted text at the bottom of this challenge description: `Gla ktwulpbr pg Klkw Urao ax qlsn{Pvelnnad Aumjcnyg: Ibrwpaty ENLECPZNYG!}`. Our objective is to decrypt that encrypted text so that we can find what happened next.

<br>
Let's input this cipher in the [Dcode Cipher Identifier](https://www.dcode.fr/cipher-identifier) to see what type of cipher this could be.

<br><img src="/assets/images/deadface_ctf_2022/two_dead_boys_3.png" alt="two_dead_boys">

<br>
Dcode suggested many different type of ciphers but every one of them has failed me. Then I started to try decrypting via different cipher algorithms. I was almost about to give up when I decided to start from the beginning. I read the poem again thinking that there might me a clue in there and there was. In the introduction, you can see that **Viginere** is written and its a cipher algorithm so I thought let's try this one.

<br><img src="/assets/images/deadface_ctf_2022/two_dead_boys_4.png" alt="two_dead_boys">

<br>
To my surprise, it decrypted the encrypted text and we got our flag.

> flag{Critical Thinking: Question EVERYTHING!}

<br>
<h1><b><u>Steganography</u></b></h1>

<br>
## The Goodest Boy

<br><img src="/assets/images/deadface_ctf_2022/the_goodest_boy_1.png" alt="the_goodest_boy">

<br>
We are given an image of a dog. Our objective is find any hidden message in the image which is hidden by one of the DEADFACE's group member. I ran the **file** command first and figured out that this is a `JPEG`file. 

<br><img src="/assets/images/deadface_ctf_2022/the_goodest_boy_2.png" alt="the_goodest_boy">

<br>
I viewed the output of the `exiftool` tool but nothing seemed useful to me which could give any clue on where the message could be hidden. I then ran the `strings` hoping that it might give me something useful and it did give.

<br><img src="/assets/images/deadface_ctf_2022/the_goodest_boy_3.png" alt="the_goodest_boy">

<br>
I found a password using the `strings` command but I don't know yet where I can apply/use this password so I still have to figure that out. I then got a clue from the **Ghost Town** thread mentioned in the challenge description. It says that `bumpyhassan` has used a windows steg tool and I remember one tool that is windows based and its name is **Steghide**.

<br><img src="/assets/images/deadface_ctf_2022/the_goodest_boy_4.png" alt="the_goodest_boy">

<br>
Next, I am going to extract any files (if present) in the image file using the `steghide` tool. I ran the following command to extract the hidden files from the image and I passed the password **borkbork** that we got from the `strings` command.
```
steghide --extract -sf doggo.jpg

--extract: extract data
-sf: extract data from doggo.jpg 
```

<br><img src="/assets/images/deadface_ctf_2022/the_goodest_boy_5.png" alt="the_goodest_boy">

<br>
It extracted a hidden PDF file. Open the PDF file and flag will be there.

<br><img src="/assets/images/deadface_ctf_2022/the_goodest_boy_6.png" alt="the_goodest_boy">

> flag{whos_A_g00d_boi_bork_bork}

<br>
## Life's A Glitch

<br><img src="/assets/images/deadface_ctf_2022/lifes_a_glitch_1.png" alt="lifes_a_glitch">

<br>
In this challenge, we are give a GIF file and our objective is to find the hidden flag in the GIF file but first things first, let's run the file command and see what type of file it is.

<br><img src="/assets/images/deadface_ctf_2022/lifes_a_glitch_2.png" alt="lifes_a_glitch">

<br>
I ran the `strings` command and `exiftool` tool but couldn't find anything useful. Although, `exiftool` told me that this file has been edited in the adobe photoshop so that tells me the flag is there in the file. So next, I ran the `stegsolve` tool to see if the flag has been hidden in the RGB values.

<br><img src="/assets/images/deadface_ctf_2022/lifes_a_glitch_3.png" alt="lifes_a_glitch">

<br>
I looked through different planes but couldn't find anything useful. So for the next step, I decided to review each frame of the file by going through the **Frame Browser** in the `stegsolve` tool.

<br><img src="/assets/images/deadface_ctf_2022/lifes_a_glitch_4.png" alt="lifes_a_glitch">

<br>
There are a total of 80 frames in the GIF file so I have to go through each frame one by one. Hard work payed off!! I found the flag on the 23rd frame of the GIF file.

<br><img src="/assets/images/deadface_ctf_2022/lifes_a_glitch_5.png" alt="lifes_a_glitch">

> flag{c0rrupt3d!}

<br>
**Those who made it to the end, I hope you enjoy this writeup. Till the next one, До встречи.**