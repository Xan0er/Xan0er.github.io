---
layout: post
title: "Hacktivitycon CTF 2021 Writeups"
date: 2021-09-18 17:15:00 +0530
categories: Infosec
tags: ctf, hacktivityconctf2021
---

Hi, hope you are doing well and welcome to another writeup of another awesome CTF. I participated in the Hactivitycon CTF 2021 this week and luckily I was able to solve some of the challenges of the CTF. So enjoy the writeups and if you have any feedback for me, you can reach out to me on my discordor twitter.

## Six Four Over Two (Warmup)

<br>
<img src="/assets/images/hacktivitycon2021/sixfour1.png" alt="Six Four Over Two">

<br>
In this challenge, we are given a text file and that text file contains a cipher text which looks like below:
```
EBTGYYLHPNQTINLEGRSTOMDCMZRTIMBXGY2DKMJYGVSGIOJRGE2GMOLDGBSWM7IK
```

After looking at this cipher, and the title of the challenge indicates that this is probably a **Base2** encoded cipher. So, I went to the <http://dcode.fr> website and searched the for Base32 decoder and it existed! After that, I tried to decode this cipher and bam, I got the flag!

<br>
<img src="/assets/images/hacktivitycon2021/sixfour2.png" alt="Six Four Over Two">

Therefore, the flag for this challenge is:
<blockquote>
<p>flag{a45d4e70bfc407645185dd9114f9c0ef}</p>
</blockquote>

<br>
## Tsunami (Warmup)

<br>
<img src="/assets/images/hacktivitycon2021/tsunami1.png" alt="Tsunami">

<br>
In this challenge, we are given an audio file with a format of **wav**. Whenever I receive a audio file, the first thing which I do is play it. After playing the audio, I didn't find anything useful. So the next step is to analyze the file with an audio analyzer software. I prefer to user **Sonic Visualizer** but you can use **Audacity** as well. I loaded the file and opened it in sonic visualizer.

<br>
<img src="/assets/images/hacktivitycon2021/tsunami2.png" alt="Tsunami">

<br>
After opening this file, the next thing I always do is add a spectogram layer to the audio so that if there is any hidden text in there it will be visible to us. After adding the spectogram layer, it will look like below:

<br>
<img src="/assets/images/hacktivitycon2021/tsunami3.png" alt="Tsunami">

<br>
And there is our flag!

<br>
<img src="/assets/images/hacktivitycon2021/tsunami4.png" alt="Tsunami">

<br>
Therefore, the flag for this challenge is:
<blockquote>
<p>flag{f8fbb2c761821d3af23858f721cc140b}</p>
</blockquote>

<br>
## Confidentiality (Web)

<br>
<img src="/assets/images/hacktivitycon2021/confidentiality1.png" alt="Confidentiality">

In this challenge, we are provided with a website which looks like below:

<br>
<img src="/assets/images/hacktivitycon2021/confidentiality2.png" alt="Confidentiality">

<br>
There is a input field on the website which takes an input and returns the list of files in a particular location. For example, if we'll enter **/etc**, it will return the files list in the **/etc** directory.

<br>
<img src="/assets/images/hacktivitycon2021/confidentiality3.png" alt="Confidentiality">

<br>
As you can see in the above image, it is returning the files list. If you want to see which command is being run the background to list all these files, we can input a wrong character.

<br>
<img src="/assets/images/hacktivitycon2021/confidentiality4.png" alt="Confidentiality">

<br>
As you can see, the command which is being run is **ls -l**. As per my intution, the flag will be most probably in the home directory of the user. So, lets' print all files in the home directory of the user.

<br>
<img src="/assets/images/hacktivitycon2021/confidentiality5.png" alt="Confidentiality">

<br>
My guess was correct. The flag file is present in the home directory of the user. Therefore, now lets print this flag.txt file by piping the commands togther as below:
```
/home/user | cat /home/user/flag.txt
```

And this gave us the flag. Therefore, the flag for this challenge is:
<blockquote>
<p>flag{e56abbce7b83d62dac05e59fb1e81c68}</p>
</blockquote>

<br>
## Swaggy (Web)

<br>
<img src="/assets/images/hacktivitycon2021/swaggy1.png" alt="Swaggy">

<br>
In this challenge, we are provided with a website which handles an API for a service.

<br>
<img src="/assets/images/hacktivitycon2021/swaggy2.png" alt="Swaggy">

<br>
Firstly, let's authorize to this API by clicking on the Authorize button creating credentials. After you have authorized, click on the **Try it out** button and then click on execute. After you execute it, you will be provided with the flag.

<br>
<img src="/assets/images/hacktivitycon2021/swaggy3.png" alt="Swaggy">

<br>
Therefore, flag for this challenge is:
<blockquote>
<p>flag{e04f962d0529a4289a685112bf1dcdd3}</p>
</blockquote>




