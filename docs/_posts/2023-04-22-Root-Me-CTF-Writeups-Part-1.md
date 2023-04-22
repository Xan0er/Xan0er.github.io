---
layout: post
title: "Root Me CTF Writeups (Part 1)"
date: 2023-04-22 12:44:05 +0530
categories: CTF
tags: ctf root-me network-challenges
---

In today's writeup, I solved some network challenges of the [Root Me](https://www.root-me.org/) platform which is a great platform to practice your hacking and infosec skills. I would recommend it everyone who's a beginner to atleast give it a try. It has challenges from beginner to advance difficulties. I hope you'll get to learn from this writeup. I am also writing this writeup for beginner's so that if they ever get stuck anywhere they can refer to my writeup. Let's get started now. Enjoy!

<br>
<h1><b><u>FTP - Authentication</u></b></h1>

<br><img src="/assets/images/2023/root_me_ctf_part_1/ftp_authentication_1.png" alt="ftp_authentication">

<br>
In this challenge, we are given a `.pcap` file which is also called **Packet Capture** file. As per the challenge, a file has been received through the FTP protocol so our objective is to recover the password used by the user for the FTP authentication. Let's open the file in the **Wireshark** program and start analyzing.

<br><img src="/assets/images/2023/root_me_ctf_part_1/ftp_authentication_2.png" alt="ftp_authentication">

<br>
I filtered out the FTP packets and followed the **TCP Stream** of the first packet. I found the username and password used for the FTP authentication.

<br><img src="/assets/images/2023/root_me_ctf_part_1/ftp_authentication_3.png" alt="ftp_authentication">

> Flag: cdts3500

<br>
<h1><b><u>TELNET - Authentication</u></b></h1>

<br><img src="/assets/images/2023/root_me_ctf_part_1/telnet_authentication_1.png" alt="telnet_authentication">

<br>
Just like the previous challenge, we are given `.pcap` file in the challenge as well. Let's fire up the **Wireshark** and fulfill our objective which is to user password on this TELNET session capture.

<br><img src="/assets/images/2023/root_me_ctf_part_1/telnet_authentication_2.png" alt="telnet_authentication">

<br>
I filtered out the TELNET packets using the `telnet` filter and followed the **TCP Stream** of the first packet. 

<br><img src="/assets/images/2023/root_me_ctf_part_1/telnet_authentication_3.png" alt="telnet_authentication">

<br>
I found the password inside the TCP packet stream.

<br><img src="/assets/images/2023/root_me_ctf_part_1/telnet_authentication_4.png" alt="telnet_authentication">

> Flag: user

<br>
<h1><b><u>ETHERNET - frame</u></b></h1>

<br><img src="/assets/images/2023/root_me_ctf_part_1/ethernet_frame_1.png" alt="ethernet_frame">

<br>
In this challenge, we are given a **.txt** file and our objective is to figure out the confidential data in this ethernet capture. The contents of the capture is mentioned below:
```
00 05 73 a0 00 00 e0 69 95 d8 5a 13 86 dd 60 00
00 00 00 9b 06 40 26 07 53 00 00 60 2a bc 00 00
00 00 ba de c0 de 20 01 41 d0 00 02 42 33 00 00
00 00 00 00 00 04 96 74 00 50 bc ea 7d b8 00 c1
d7 03 80 18 00 e1 cf a0 00 00 01 01 08 0a 09 3e
69 b9 17 a1 7e d3 47 45 54 20 2f 20 48 54 54 50
2f 31 2e 31 0d 0a 41 75 74 68 6f 72 69 7a 61 74
69 6f 6e 3a 20 42 61 73 69 63 20 59 32 39 75 5a
6d 6b 36 5a 47 56 75 64 47 6c 68 62 41 3d 3d 0d
0a 55 73 65 72 2d 41 67 65 6e 74 3a 20 49 6e 73
61 6e 65 42 72 6f 77 73 65 72 0d 0a 48 6f 73 74
3a 20 77 77 77 2e 6d 79 69 70 76 36 2e 6f 72 67
0d 0a 41 63 63 65 70 74 3a 20 2a 2f 2a 0d 0a 0d
0a
```

<br>
I created a small python script to decode the above *hex* bytes data to simple *ASCII* bytes data.
```
import binascii

eth_frame = open("./ETHERNET - frame.txt").read()
bytes_list = " ".join(eth_frame.splitlines()).split(" ")

decoded_str = b""
for byte_ in bytes_list:
    decoded_str += binascii.unhexlify(byte_)

print(decoded_str)
```

<br>
The result of the above python script is:
```
b'\x00\x05s\xa0\x00\x00\xe0i\x95\xd8Z\x13\x86\xdd`\x00\x00\x00\x00\x9b\x06@&\x07S\x00\x00`*\xbc\x00\x00\x00\x00\xba\xde\xc0\xde \x01A\xd0\x00\x02B3\x00\x00\x00\x00\x00\x00\x00\x04\x96t\x00P\xbc\xea}\xb8\x00\xc1\xd7\x03\x80\x18\x00\xe1\xcf\xa0\x00\x00\x01\x01\x08\n\t>i\xb9\x17\xa1~\xd3
GET / HTTP/1.1\r\nAuthorization: Basic Y29uZmk6ZGVudGlhbA==\r\n
User-Agent: InsaneBrowser\r\n
Host: www.myipv6.org\r\n
Accept: */*\r\n\r\n'
```

<br>
In the decoded result of the frame hex bytes, you can see that there is a **Authorization** header and it contains some kind of authorize key which is being used to authenticate the frame. The authorization key (`Y29uZmk6ZGVudGlhbA==`) is encoded with the **base64** encoding. So, our next step is to decode this string.

<br><img src="/assets/images/2023/root_me_ctf_part_1/ethernet_frame_2.png" alt="ethernet_frame">

<br>
After decrypting, I get a string separated by comma as shown in the image above which looks like our confidential data.

> Flag: confi:dential

<br>
<h1><b><u>Twitter Authentication</u></b></h1>

<br><img src="/assets/images/2023/root_me_ctf_part_1/twitter_authentication_1.png" alt="twitter_authentication">

<br>
A `.pcap` file has been provided to us in this challenge. A twitter session has been captured in this file and our objective is to find the password of the twitter account that was used to login to the twitter. Let's opne up **Wireshark** and start exploring.

<br><img src="/assets/images/2023/root_me_ctf_part_1/twitter_authentication_2.png" alt="twitter_authentication">

<br>
As you can see, there is only one packet that has been captured in this file. Let's follow **TCP Stream** and see if we can find anything interesting.

<br><img src="/assets/images/2023/root_me_ctf_part_1/twitter_authentication_3.png" alt="twitter_authentication">

<br>
We have found quite a few details in this packet. There is a twitter session token and there is a Authorization token.
```
Authorization: Basic dXNlcnRlc3Q6cGFzc3dvcmQ=
```

<br>
By looking at the above authorization header, it is using **Basic Authorization** technique and the string looks like **base64** encoded string so let's decode it and see if we can find the username and password from this.

<br><img src="/assets/images/2023/root_me_ctf_part_1/twitter_authentication_4.png" alt="twitter_authentication">

<br>
After decrypting the string, we got our username and password.

> Flag: password

<br>
<h1><b><u>Bluetooth - Unknown file</u></b></h1>

<br><img src="/assets/images/2023/root_me_ctf_part_1/bluetooth_unknown_file_1.png" alt="bluetooth_unknown_file">

<br>

This challenge is different from the previous challenges because in this challenge, we are given a `.bin` file. This file could contain any type of data so first of all, we'll have to determine which type of file it is and which type of data it contains. I will use the `file` command to know more about this file.

<br><img src="/assets/images/2023/root_me_ctf_part_1/bluetooth_unknown_file_2.png" alt="bluetooth_unknown_file">

<br>
So, now we know that this is a **BTSnoop version 1, HCI UART (H4)** file and after a google search, I found out that this file contains Bluetooth HCI traffic. Also, I figured out that this file can be opened with **Wireshark** for analysis. Our obejctive is to find the device name and the MAC address of the device.

<br><img src="/assets/images/2023/root_me_ctf_part_1/bluetooth_unknown_file_3.png" alt="bluetooth_unknown_file">

<br>
Since this was my first time dealing with this type of file, I started browing through each of the packets manually since there are only 27 packets in this file. 

<br><img src="/assets/images/2023/root_me_ctf_part_1/bluetooth_unknown_file_4.png" alt="bluetooth_unknown_file">

While I was analyzing the first packet, I found the device name and the MAC address of the device under the **Bluetooth HCI Event** section.
```
Device Name - GT-S7390G
MAC Address - 0C:B3:19:B9:4F:C6
```

<br>
Now, as per the challenge description, need to submit the sha1 of the string `0C:B3:19:B9:4F:C6GT-S7390G`. So, I used the [SHA1 Online](http://www.sha1-online.com/) webiste to find the sha1 of the string.

<br><img src="/assets/images/2023/root_me_ctf_part_1/bluetooth_unknown_file_5.png" alt="bluetooth_unknown_file">

> Flag: c1d0349c153ed96fe2fadf44e880aef9e69c122b

<br>
<h1><b><u>CISCO - password</u></b></h1>

<br><img src="/assets/images/2023/root_me_ctf_part_1/cisco_pass_1.png" alt="cisco_pass">

<br>
In this challenge, we are given a configuration file of a CISCO router maybe (I'm not sure). Our objective is to find the enable password. So, at first glance, I reviewed the file and I saw a hash in the configuration file in front of **enable** keyword so I thought that that would be the password once this hash is cracked.
```
enable secret 5 $1$p8Y6$MCdRLBzuGlfOs9S.hXOp0.
```

<br>
But, this is not a valid hash. I then saw some other hash values as well in the configuration file.
```
username hub password 7 025017705B3907344E 
username admin privilege 15 password 7 10181A325528130F010D24
username guest password 7 124F163C42340B112F3830
```

<br>
After some researching on google, I found out the there is a utilitiy to crack these hashes easily. The name of this utility is [**IFM - CISCO Password Cracked**](https://www.ifm.net.nz/cookbooks/passwordcracker.html). So, I started using this utility to crack the passwords.

<br><img src="/assets/images/2023/root_me_ctf_part_1/cisco_pass_2.png" alt="cisco_pass">

<br><img src="/assets/images/2023/root_me_ctf_part_1/cisco_pass_3.png" alt="cisco_pass">

<br><img src="/assets/images/2023/root_me_ctf_part_1/cisco_pass_4.png" alt="cisco_pass">

<br>
After cracking all the hashes, I started noticing a pattern in the passwords that the string before the underscore remains the same and the string after the underscore is changing.
```
6sK0_hub
6sK0_admin
6sK0_guest
```

<br>
So, I thought that the **enable** password must follow these same rules as the other passwords. Therefore, I tried `6sK0_enable` as password and it worked.

> Flag: 6sK0_enable

<br>
**I hope learned something new while reading these writeups. Stay tuned for the next part !!!**