# 17 Oct , 2026

8:02 AM
This is my first box which I'm trying to do without any help from anything (No write ups , No AI , No YouTube , Nothing).
I will do this own my own. (Unless i get stuck(which i do))

I am given an IP and i only know two things to do with it
1. nmap
2. open it in browser

The browser part is not going too well as it's not opening any website or anything , all i can do is for my nmap scan to finish. My nmap scan is going on for a very long time

Even running curl on the given IP returns `curl: (56) Recv failure: Connection reset by peer` , i may have a long session for this
The browser failed to load the ip too.

My nmap scan is finished , and i can see i have 3 ports to work with
1. 21 , ftp , version = vsftpd 3.0.3
2. 22 , ssh , version = OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0 (some ssh host keys are listed too)
3. 80 , http , Gunicorn

With 3 , i can assume this box is a server machine. Now i will try my options of ftp and ssh.

It seems like ftp does not support anonymous logins , even trying USER inside the ftp environment with anonymous didnt work.
ssh wants my user's password on the host machine , and i do not know my password.

The ip i used for opening in the browser returns a blank page , so no site opens with it.

8:23 AM
I will now switch to the hint mode of cap , i think i've hit a wall.
Task 2 assumes that i have the website open , am i doing something wrong ?
I will open the write up of Cap just to see the next step.
Even in the write up , after the same discovery of FTP that i made , he just casually opens the site. I have to be doing something wrong.
Neither the given ip , or ip:80 opens anything on the browser.

I will now ask Gemini for this specific step's error and fix.
Prompting it with "In the Cap machine in HTB , the ip im given does not open a website in my browser , why is that the case (Do not spoil anything else about the Box , just this issue)".
One solution it gave that i thought would work was explicitly writing http:// in the url bar , but that doesnt seem to do anything , so now i just have to verify my OpenVPN connectivity.
Ping was also successful , so i try even more steps.
curl http://10.129.174.32 , is also not giving me any result , it truly has to be an error with some connection.

I FOUND THE FIX , i had to change my mtu value with "sudo ip link set dev tun0 mtu 1200" , to allow webpage transfers , AND IT WORKED.

8:40 AM
Now that i got it running , i close all my help (still in guided mode though) , and continue.
The website has a "security snapshot" thing which gives me a near empty pcap file for wireshark. HTB is asking if i can get other people's scan from this which for now i dont think so.
HTB hints that i should fuzzy find the url which will let me look at other user's scan with `ffuf`.
I have a feeling i am using ffuf wrong.

8:55 AM
Again , i will look at the writeup for this step only.
Wow ok so instead of changing "data/1" to "/2 , /3" like i was doing , it was "/0". bruh
I download that pcap file with a lot of packets in it.
In the pcap , i was able to find a username and password. I will now try these in ftp , IT WORKED.
I got a user.txt text from that , AND I GOT ONE FLAG LETS GOOOO.

The same name and password for nathan worked in ssh too.

Now we need to find a file in bin which can help us get root access.
Okay , finding a file like that without any tool is hard , so i will have to AGAIN look at the writeup.

9:30 AM
This escalation thing is very weird , i'll have to watch a youtube video now.
linPEAS require me to download it , and host it in a way that me in ssh can access linpeas in my own pc. very neat.
Also turns out i needed to write linpeas_fat.sh instead of blindly following tutorial with writing linpeas.sh (i had linpeas hosted thats why)

A very big wall of text is here with linpeas. 

9:54 AM
Finally found a highlighted proccess file /usr/bin/python3.8 = cap_setuid,cap_net_bind_service+eip , this seems like my target

After running the python script , and setting my uid to 0 , i was able to read the final flag

Finished at 10:00 AM
Two damn hours , but atleast i know about linPEAS setup and how to fix a HTB website not opening.
