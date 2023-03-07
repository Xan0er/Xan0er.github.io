---
layout: post
title: "Angstrom CTF 2022 Writeups"
date: 2022-04-31 17:15:00 +0530
categories: CTF
tags: angstrom_ctf
---

Hey everyone, this week I particpated in the **Angstrom CTF 2022** and it was a blast. I wasn't able to solve many challenges but I did manage to solve some of the challenges. I ended up at 846th position during the CTF. Here is the overview of the challenges which I managed to solve.

Also, I have completed the writeup of this CTF after almost a year since I was busy so there will be some challenges which I won't be able to explain properly since those challenges URLs have been taken down by the organizers such as the Web based challenges.

<br>
<img src="/assets/images/angstrom_ctf_2022/score.png" alt="score">

<br>
<h1><b><u>Misc</u></b></h1>

<br>
## Interwebz

<br><img src="/assets/images/angstrom_ctf_2022/interwebz_1.png" alt="interwebz">

For this challenge, we only have to connect to the mentioned domain and port in the challenge description. For that, I used the **netcat** command.
```
nc challs.actf.co 31335
```

So, by running the above command, I captured the flag.
> actf{plugged_in_and_ready_to_go}

<br>
## Confetti

<br><img src="/assets/images/angstrom_ctf_2022/confetti_1.png" alt="confetti">

We are given an image in this challenge so it could be possible that the flag is hiddent inside this image. Here's a preview of the image:

<br><img src="/assets/images/angstrom_ctf_2022/confetti_2.png" alt="confetti">

I ran the **file** command first in order to find the type of the file. It's a PNG image file.

<br><img src="/assets/images/angstrom_ctf_2022/confetti_3.png" alt="confetti">

Then I passed the image through **binwalk** first to see if there was anything else attached to the file. Going back to the lyrics we were given for the description of this challenge, it mentions confetti dropping from the sky. Reminds us of a certain binwalk argument that looks like a star falling from the sky:
``` 
binwalk --dd ".*" confetti.png
```

<br><img src="/assets/images/angstrom_ctf_2022/confetti_4.png" alt="confetti">

<br><img src="/assets/images/angstrom_ctf_2022/confetti_5.png" alt="confetti">

As you can see in the above image, there are many image files extracted with the **binwalk**. So our flag should be in one of them.

<br><img src="/assets/images/angstrom_ctf_2022/confetti_6.png" alt="confetti">

We got our flag:
> actf{confetti_4_u}

<br>
## Shark 1

<br><img src="/assets/images/angstrom_ctf_2022/shark_1_1.png" alt="shark_1">

In this challenge, we are give a network capture file with extention **pcapng**. So, I have to analyze this file using a program called **WireShark** and find the flag.

<br><img src="/assets/images/angstrom_ctf_2022/shark_1_2.png" alt="shark_1">

<br><img src="/assets/images/angstrom_ctf_2022/shark_1_3.png" alt="shark_1">

Next, I followed one of the first network TCP streams to see the content which is being sent via the TCP protocol. And, I got lucky because I captured the flag.

<br><img src="/assets/images/angstrom_ctf_2022/shark_1_4.png" alt="shark_1">

> actf{wireshark_doo_doo_doo_doo_doo_doo}

<br>
## Shark 2

<br><img src="/assets/images/angstrom_ctf_2022/shark_2_1.png" alt="shark_2">

Just like the previous challenge, we are given a network packet capture file again in this challenge but our goal remains same i.e. capturing the flag. We will open this file in the **Wireshark** and analyze the network packets. 

<br><img src="/assets/images/angstrom_ctf_2022/shark_2_2.png" alt="shark_2">

In one of the TCP streams, I came across content which looks like image data since the starting bytes of the content contained the **JFIF** which is the header for the *JPEG* image type.

<br><img src="/assets/images/angstrom_ctf_2022/shark_2_3.png" alt="shark_2">

I changed the show data as from **ASCII** to **Raw** and saved the image as JPEG image.

<br><img src="/assets/images/angstrom_ctf_2022/shark_2_4.png" alt="shark_2">

We have our flag in the extracted image from the packet capture file.

<br><img src="/assets/images/angstrom_ctf_2022/shark_2_5.png" alt="shark_2">

> actf{i_see_you}

<br>
<h1><b><u>Web</u></b></h1>

<br>
## The Flash

<br><img src="/assets/images/angstrom_ctf_2022/flash_1.png" alt="flash">

In this challenge, It looks like a site where fake flags are displayed when you access it. If you look closely, the display will change for a moment. It seems that js is instantaneously replacing it with a real flag. So, I have to use my browser's features to set breakpoints on changes. 

Once I did that, I got the flag.
> actf{sp33dy_l1ke_th3_fl4sh}

<br>
## Auth Skip

<br><img src="/assets/images/angstrom_ctf_2022/auth_skip_1.png" alt="auth_skip">

For this, we have to skip the authentication part and capture the flag located on the other side of the authentication. When I try to access the web page, it looks like a login form, but since I don't have the credentials to login so I proceed further. The source code of the authentication module looks like this:
```
app.get("/", (req, res) => {
    if (req.cookies.user === "admin") {
        res.type("text/plain").send(flag);
    } else {
        res.sendFile(path.join(__dirname, "index.html"));
    }
});
```

By reviewing the above code, I came to a conclusion that I have to pass cookie **user** with value of **admin** in order to successfully by pass the authentication. I used curl for this purpose:
```
curl https://auth-skip.web.actf.co/ -H "Cookie: user=admin"
```

After sending request, I got the flag.
> actf{passwordless_authentication_is_the_new_hip_thing}

<br>
## Crumbs

<br><img src="/assets/images/angstrom_ctf_2022/crumbs_1.png" alt="crumbs">

When we visit the web page of this challenge, it gives us the url of the next webpage to follow. I used **curl** command to demonstrate how it looked on the webpage
```
curl https://crumbs.web.actf.co/
    Go to 61f57d99-6d8e-4e5e-bfc1-995dc358fce7

curl https://crumbs.web.actf.co/61f57d99-6d8e-4e5e-bfc1-995dc358fce7
    Go to 24c73741-cdd9-4c76-bf79-fb82304a6ceb

curl https://crumbs.web.actf.co/24c73741-cdd9-4c76-bf79-fb82304a6ceb
    Go to 1eb4cc3f-204b-4ba2-acd7-30d833676347
```

So, as per my intuition, we have to keep following the following until we reach the flag or we reach a url that gives us the flag. But we don't know how many URLs we have to go through since there could be 100s or 1000s of URLs so I wrote a simple script to capture the flag in python.
```
import requests

chall_url = "https://crumbs.web.actf.co/"
next_path = ""

while True:
    resp = requests.get(chall_url + next_path)
    resp_text = resp.text
    print(resp_text)
    
    if "actf{" in resp_text: # Checks for the flag format in the response text
        break
    
    next_path = text.replace("Go to ", "")
```

Thus, eventually I got the flag.
> actf{w4ke_up_to_th3_m0on_6bdc10d7c6d5}

<br>
<h1><b><u>Crypto</u></b></h1>

<br>
## Caesar and Desister

<br><img src="/assets/images/angstrom_ctf_2022/caesar_1.png" alt="caesar">

We are given an encrypted string which we have to decode in order to get the flag. There is a hint in the challenge title that this could be the **Caesar** cipher. 
```
sulx{klgh_jayzl_lzwjw_ujqhlgyjshzwj_kume}
```

<br>
So, I payed a little visit to the [Dcode Caesar Code](https://www.dcode.fr/chiffre-cesar) in order to decode this Caesar cipher.

> actf{stop_right_there_cryptographer_scum}

<br>
<h1><b><u>Rev</u></b></h1>

<br>
## Baby3

<br><img src="/assets/images/angstrom_ctf_2022/baby3_1.png" alt="baby3">

After running the **file** command, I found out that this is an **ELF 64-bit Linux Executable** file.

<br><img src="/assets/images/angstrom_ctf_2022/baby3_2.png" alt="baby3">

I ran the `strings chall` command and found the flag.

<br><img src="/assets/images/angstrom_ctf_2022/baby3_3.png" alt="baby3">

> actf{emhpaidmezerodollarstomakethischallenge_amogus}

<br>
**I hope you enjoyed reading this writeup. See you guys in the next one.**