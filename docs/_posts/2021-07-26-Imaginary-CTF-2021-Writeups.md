---
layout: post
title: "Imaginary CTF 2021 Writeups"
date: 2021-07-26 17:15:00 +0530
categories: Infosec
tags: ctf, imaginaryctf, writeups
---

Hi everyone, welcome to the Xan0er's Blog. This weekend, I participated in the ***Imaginary CTF 2021*** and learned a lot of things along with a lot of fun so I want to share that learning experience with you guys. Here are some writeups of the challenges which I was able to solve during this CTF:

## Roo's World

<br>
<img src="/assets/images/RoosWorld.png" alt="roosworld" />

<br>
In this challenge, I was given a website. After visiting the website, it saw that it was just a normal website. When I inspected the source code the website, I found the below text:

<img src="/assets/images/jsfuck.png" />

<br>
After doing some research, I found that this is cipher type text called ***JSFuck***. I found an online [JSFuck Obsicutator](https://enkhee-osiris.github.io/Decoder-JSFuck/) and after decoding this I got the following string:

```
console.log(atob("aWN0ZnsxbnNwM2N0MHJfcjAwX2cwZXNfdGgwbmt9"));
```

<br>
After some googling about this ***atob*** function, I founded that this function is used to decode a base-64 encoded string. So I copy pasted this command in the console command of the google chrome browser and I got the flag:

<blockquote><p>ictf{1nsp3ct0r_r00_g0es_th0nk}</p></blockquote>

<br>
## Hidden PSD

<br>
<img src="/assets/images/hiddenpsd.png" />

<br>
In this challenge, a photoshop file was given in format of ***PSD***. When I opened up this file using the [PhotoPea Editor](https://www.photopea.com/), it looked liked this:

<img src="/assets/images/psd1.png" />

<br>
When I removed the the layer of the red square box from the front of the flag, I got the full flag:

<img src="/assets/images/psd2.png" />

<br>
So the flag for this challenge is:

<blockquote><p>ictf{wut_how_do_you_see_this}</p></blockquote>

<br>
## Chicken Caesar Salad

<br>
<img src="/assets/images/ChickenCaesorSalad.png" />

<br>
In this challenge, we are provided with a text file and it contains the following text:

```
qkbn{ePmv_lQL_kIMamZ_kQxpMZa_oMb_aW_pIZl}
```

<br>
On the basis of the challenge title and some google research, I found that this is a caesar cipher so I went to the <https://www.dcode.fr/caesar-cipher> and decoded this cipher. So the flag for this challenge is:

<blockquote><p>ictf{wHen_dID_cAEseR_cIphERs_gEt_sO_hARd}</p></blockquote>

<br>
I was only able to solve these challenges since I didn't got enough time to solve the other challenges. Next time, I will try harder. Thank you guys for reading my writeups. Stay tuned for more upcoming writeups!
