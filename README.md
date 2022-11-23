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


