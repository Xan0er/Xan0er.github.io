---
layout: post
title: "PB Jar CTF 2021 Writeups"
date: 2021-09-20 17:15:00 +0530
categories: Infosec
tags: ctf pbjar-ctf 2021
---

I am back with another writeup of the CTF which I played this week. It was the **PBJar CTF 2021** and I had a lot of fun while playing this CTF. I was able to solve some of the challenges from this CTF and I hope that you will like these challenges writeups so here we go!

## Convert (Crypto)

<br>
<img src="/assets/images/pbjar2021/convert1.png" alt="Convert">

<br>
In this challenge, we are given a zip file and when we unzip that zip file. We get the following content:
```
666c61677b6469735f69735f615f666c346767675f68317d
```
So, all we have to do now is to convert the above *Hex* code into *ASCII* code and according to the challenge description, then we will get our  flag so let's try that.

<br>
I fired up my **python** terminal and called the *decode()* method and bam it returned my flag:
```
>>> bytearray.fromhex("666c61677b6469735f69735f615f666c346767675f68317d").decode()
```
<img src="/assets/images/pbjar2021/convert2.png" alt="Convert">

<br>
And here it is our flag:
<blockquote>
<p>flag{dis_is_a_fl4ggg_h1}</p>
</blockquote>

<br>
## Miner (Miscellaneous)

<br>
<img src="/assets/images/pbjar2021/miner1.png" alt="Miner">

<br>
This is the first time I played this typed of challenge and I really enjoyed it. In this challenge, we'll have to find the address of a miner who validated a ethereum block by using a service present online. So, for solving this, I first went to the **[Ethereum Explorer](https://etherscan.io/)**.

<br>
<img src="/assets/images/pbjar2021/miner2.png" alt="Miner">

<br>
After that, I searched for the block number **11834380** as given in the challenge description and I got the transactions for that block including the address of the miner who validated that block.

<br>
<img src="/assets/images/pbjar2021/miner3.png" alt="Miner">

<br>
As you can see in the above image, the address of the miner is given. Now, we just need to wrap it in the **flag{}** format. Therefore, our flag is:
<blockquote>
<p>flag{0xd224ca0c819e8e97ba0136b3b95ceff503b79f53}</p>
</blockquote>

<br>
## readFlag1 (Miscellaneous)

<br>
<img src="/assets/images/pbjar2021/readflag1_1.png" alt="readflag1">

<br>
Here is another challenge related to the cryptocurrency technology. In this challenge, we are given a smart contract ID and we'll have to find the flag inside it. Also, it is mentioned where we can access this smart contract. So, firstly, I am going to visit this given smart contract explorer.
> [Ropsten](https://ropsten.etherscan.io/)

<br>
<img src="/assets/images/pbjar2021/readflag1_2.png" alt="readflag1">

<br>
After visiting this explorer, I searched for the smart contact ID as given in the challenge description: **0xf0674CD7D1C0c616063a786E7d1434340E09BadD** and I got the following result:

<br>
<img src="/assets/images/pbjar2021/readflag1_3.png" alt="readflag1">

<br>
And as you can see in the above image(did you saw it?!), we have our flag in the **Contract Source Code**. Therefore, here is the flag for this challenge:
<blockquote>
<p>flag{etherscan_S0urc3_c0de}</p>
</blockquote>

<br>
## ReallynotSecureAlgorithm (Crypto)

<br>
<img src="/assets/images/pbjar2021/rsa1.png" alt="rsa">

<br>
This is RSA algorithm challenge. In this challenge, we have to crack the rsa cipher by using the following values as provided in the challenge secription and I have written those for you below:
```
194522226411154500868209046072773892801
288543888189520095825105581859098503663
65537
2680665419605434578386620658057993903866911471752759293737529277281335077856
```

By notating the above numbers, they'll become:
```
p = 194522226411154500868209046072773892801
q = 288543888189520095825105581859098503663
e = 65537
c = 2680665419605434578386620658057993903866911471752759293737529277281335077856
```

<br>
For those of you who don't know about the above values, these are the values which are use to encrpyt and decrypt the RSA algorithm. Here, 
```
p and q are prime numbers
e is just a number
c is the encryted cipher text
```

<br>
Follow below steps to decrypt this cipher:
   1. Calculate the **n** value:<br>
		n = p * q

	2. Calculate the **phi** value:<br>
		phi = (p-1) * (q-1)
		
	3. Next is calculate the inverse of **e mod phi**:<br>
		d = inverse(e, phi)
		
	4. Decode the cipher text:<br>
		m = pow(c,d,n)
		
	5. Decode the above cipher text from the hex values:<br>
		print(repr(binascii.unhexlify(hex(m)[2:])))

<br>
The flag for this challenge is:
<blockquote>
<p>flag{n0t_to0_h4rd_rIt3_19290453}</p>
</blockquote>

<br>
## readFlag3 (Miscellaneous)

<br>
<img src="/assets/images/pbjar2021/readflag3_1.png" alt="readflag3">

<br>
This challenge is the third part of the **readFlag** series. Just like in the **readFlag1** challenge, we are given a smart contract ID **0xe2a9e67bdA26Dd48c8312ea1FE6a7C111e5D7a7A**. So, let's open this contract ID in **[Ropsten](https://ropsten.etherscan.io/)**.

<br>
<img src="/assets/images/pbjar2021/readflag3_2.png" alt="readflag3">

<br>
Under the contract tab, you will find the **Constructor Arguments** and as in the above image you can see that the flag is being set inside a contructor. Therefore, the flag for this challenge is:
<blockquote>
<p>flag{s3t_by_c0nstructor}</p>
</blockquote>

<br>
## Polymer (Rev)

<br>
<img src="/assets/images/pbjar2021/polymer1.png" alt="polymer">

<br>
In this challenge, we are given a binary file. When we run this binary file, it prints a flag in each line and it takes a lot of time to get to the right flag.

<br>
<img src="/assets/images/pbjar2021/polymer2.png" alt="polymer">

<br>
So in order to solve this problem, what I did is that, I saved the output of the `strings polymer` to a file. And, I saw a lot of similar flags in the file by the name of `flag{n0t_th3_fl4g_l0l}`.

<br>
<img src="/assets/images/pbjar2021/polymer3.png" alt="polymer">

<br>
I removed the flag which is repeating a lot of time with the help of **Sublime Text** editor. And the only flag which was not repeating is shown in the below image.

<br>
<img src="/assets/images/pbjar2021/polymer4.png" alt="polymer">

<br>
The flag for this challenge is:
<blockquote>
<p>flag{ju5t_4n0th3r_str1ng5_pr0bl3m_0159394921}</p>
</blockquote>

<br>
## cOrL (Web)

<br>
<img src="/assets/images/pbjar2021/corl1.png" alt="corl">

<br>
I was only able to solve only one web challenges in this CTF. In this challenge, we are given a link to a login portal. I tried login in to this portal but it throwed an error saying that I am not an admin. So after that, I tried to login with the most common credentials: `admin:admin`.

<br>
<img src="/assets/images/pbjar2021/corl2.png" alt="corl">

<br>
Now, it gave another error as shown in the image above which says that **the admin must have put some additional security protections here** in whch the **put** is highlighted in bold which gave an idea that maybe I should intercept this request in burp suite and try to change the **HTTP method** of this login request. So I fired up my burp.

<br>
<img src="/assets/images/pbjar2021/corl3.png" alt="corl">

<br>
After intercepting the request, I sent that request to the repeater as shown in the image above and I changed the **POST** method to the **PUT** method. And luckily, I was able to login as admin and I got the flag as shown below.

<br>
<img src="/assets/images/pbjar2021/corl4.png" alt="corl">

<br>
Therefore, the flag for this challenge is:
> flag{HTTP_r3qu35t_m3th0d5_ftw}

<br>
## Hack NASA With HTML Mr. Inspector Sherlock (Web)

<br>
<img src="/assets/images/pbjar2021/sherlock1.png" alt="sherlock">

<br>
In this challenge, we are given a website in which the flags are distributed in 3 parts. We have to check the source code of the website and find the flags. The first part of the flag can be found in the **animate.js** file: `flag{wA1t_a_m1nUt3_`.

<br>
<img src="/assets/images/pbjar2021/sherlock2.png" alt="sherlock">

<br>
The second part of the flag can be found in the **what.html** file: `I_th0ugh1_sh3l0ck_w2s`.

<br>
<img src="/assets/images/pbjar2021/sherlock3.png" alt="sherlock">

<br>
The third part of the flag can be found in the **index.html**: `_a_d3t3ct1iv3????!?!?!}`.

<br>
<img src="/assets/images/pbjar2021/sherlock4.png" alt="sherlock">

<br>
Therefore, the full flag is:
> flag{wA1t_a_m1nUt3_I_th0ugh1_sh3l0ck_w2s_a_d3t3ct1iv3????!?!?!}

<br>
**Thank you reading my writeup and stay tuned for the next one!**
