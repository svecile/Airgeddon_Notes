# Airgeddon
[Airgeddon](https://github.com/v1s1t0r1sh3r3/airgeddon) is a penatration testing tool made to perform a variety of attacks against wireless access points. These attacks can be used by penatration testers to audit the security of wireless access points or networks. Airgeddon is a bash script that brings together many standalone hacking tools that you may or may not be aware of into one convienient interface.

Some of the more popular tools that are used by the script are:

- [Aircrack-ng](https://www.aircrack-ng.org/)
- [Hashcat](https://hashcat.net/hashcat/)
- [BeEF](https://beefproject.com/)
- [John the Ripper](https://www.openwall.com/john/)
- [Bully](https://github.com/kimocoder/bully)
- [Reaver](https://github.com/t6x/reaver-wps-fork-t6x)
- [bettercap](https://www.bettercap.org/intro/)
- [tshark](https://tshark.dev/)

These are just the more well known tools it uses you can see the essential tools [here](https://github.com/v1s1t0r1sh3r3/airgeddon/wiki/Essential%20Tools) and the optional tools [here](https://github.com/v1s1t0r1sh3r3/airgeddon/wiki/Optional%20Tools). These tools all combine to make airgeddon a powerful tool that includes [functionality](https://github.com/v1s1t0r1sh3r3/airgeddon/wiki/Features) for:
- WPA/WPA2
	- personal network handshake and PMKID capturing
- Offline password decription
	- dictionary, bruteforce or rule based attacks
- Evil Twin attacks (rogue access point)
	- Create a fake access point to spoof a real one then get unsuspecting users to connect to it
	- Create captive portal to phish for the real networks password
	- Once connected use ssl strip to downgrade security and sniff network packets for passwords and other sensitive information
- WPS
	- WPS scanning
	- Pixie dust attacks
	- PIN bruteforcing
	- Known PIN attacks
	- Null PIN attack
- Enterprise Network Attacks
	- Create evil twin access point to capture enterprise credentials
	- Smooth and noisey modes to evade detection or to just hammer the network
- WEP all-in-one attack
	- If you would like to know more about WEP and WEP cracking [here's](https://null-byte.wonderhowto.com/how-to/hack-wi-fi-hunting-down-cracking-wep-networks-0183712/) an article from Null Byte Wonder How To, that uses aircrack-ng

As you can see airgeddon has a ton of functionality, wether your auditing your home WiFi or large enterprise networks airgeddon has the capability to fit your needs. Unfortunatly because it has so much functionality I wont be able to go over it all in 10 minutes so i will focus on personal network WPA/WPA2 handshake attacks, WPS attacks and Evil Twin attacks because I think they are the most relivant to you, I am able to set up a personal network for a demo and i think they are just fun.

## So How Do We Go About Hacking WiFi networks?

First things first, your going to need the proper equiptement in terms of software and hardware.

### Software
I will be using a [VirtualBox](https://www.virtualbox.org/) version of [Kali Linux](https://www.kali.org/) which can be found [here](https://www.kali.org/get-kali/#kali-virtual-machines). You will also need to [install airgeddon](https://github.com/v1s1t0r1sh3r3/airgeddon/wiki/Installation%20&%20Usage#generic-installation) onto kali which can be done by entering three simple commands into the terminal.
`1. git clone https://github.com/v1s1t0r1sh3r3/airgeddon.git`
`2. cd airgeddon`
`3. sudo bash airgeddon.sh`
Once the third command is run the airgeddon script will start and check if you have all the nessicary packages installed and if you dont it will ask if you want it to install them for you, to which you should say yes.

Airgeddon can also be used on other operating systems such as Windows or Mac OSX using docker and you can find instructions for that setup [here](https://github.com/v1s1t0r1sh3r3/airgeddon/wiki/Docker). It can also be used in much the same way as with kali on other [compadable Linux distrobutions](https://github.com/v1s1t0r1sh3r3/airgeddon/wiki/Compatibility).

### Hardware
To run attacks against WiFi networks you need to have a WiFi adapter that can do packet injection so most likley the WiFi adapter you have in your laptop or computer won't work. Specifically its the wireless chipset within the adapter that will determine if it can do packet injection or not. A list of compadable cards and chipsets can be found [here](https://github.com/v1s1t0r1sh3r3/airgeddon/wiki/Cards%20and%20Chipsets).
I personally have the Alpha AWUS036NHA with the Atheros AR9271 chipset, and the Alpha AWUS036ACM with the MediaTek MT7612U chipset.
##### Alpha AWUS036NHA
<img src="https://github.com/svecile/Airgeddon_Notes/blob/main/Alfa%20AWUS036NHA.jpg" alt="AWUS036NHA"/>
Shown here is the Alpha AWUS036NHA usb WiFi adapter with the Atheros AR9271 chipset. It is by far the most popular and widely compadable adapter. Attached to it is the 5 dBi antenna that it came with and next to it is a 9 dBi antenna that i got off amazon. The larger antenna is not required although it does help a lot with range if you are not close to the WiFi network you are attacking.  I bought this adapter many years ago and it still works great today. However, when using airgeddon with it i was having trouble capturing handshake files and after a while i realised it was not anything to do with airgeddon or the adapter but it was because this adapter is only capable of using 2.4 GHz WiFi. My router and many newer routers connect devices using the 5 Ghz WiFi frequency by default which was not the case 7 years ago when i bought the adapter. Back then 2.4 GHz was the default with the option to turn on 5 GHz. So while this adapter is really good, it is starting to show its age and wont help you with penatration testing many newer networks unless they have some older devices connected on 2.4 GHz. My network had no such devices and instead of turning off 5 GHz and pissing off all my roommates while their devices reconnect, i dusted off my old iPhone 4 which doesn't have 5 GHz capabilities to use in initial testing.

##### Alpha AWUS036ACM
<img src="https://github.com/svecile/Airgeddon_Notes/blob/main/Alfa%20AWUS036ACM.jpg" alt="AWUS036ACM" width="200"/>
Due to the shortcomings of the previous adapter i decided to get a newer one with 2.4 and 5 GHz capabilities and the one i chose was the Alpha AWUS036ACM with MediaTek MT7612U chipset. This adapter is listed as having problems with kali virtual machines in the airgeddon compadability list but i didnt run into those issues and i read in a couple forums with newer versions of kali it isnt an issue anymore.  This adapter worked excelent and also i found had more than adiquate range without upgrading the antennas.

---
### Network Recon
Next you are going to need to do some recon and figure out the type of network you are dealing with and what kind of security it has so you can attack it. To perform wireless recon you will have to put your WiFi adapter in monitor mode which is done by typing 2 on the main airgeddon screen. There are two types of networks you will encounter personal and enterprise.

#### Enterprise Network
It will be obvious if you are dealing with an enterprise network because when you click to join the network it will ask you for a username and password. An example of this would be our university WiFi uwosecure-v2. These networks are much larger than your home network and consist of many physical and virtual networks and protocols that connect users and devices to a shared local area network (LAN). Devices on these networks are connected together using ethernet, WiFi, switches and routers so they can share data securely within the network. Airgeddon has enterprise attack functionality which is option 10 on the main menu.

#### Personal Network
This is the type of network we are going to focus on in this presentation. It is the most common type of network and is what your home WiFi would be characterised as. It requires only a password which is in contrast to a username and password combination that is needed for enterprise networks. These networks are often comprised of a router and possibly some WiFi extender pods.

For demostration pourposes in the presentation i will be using the personal hotspot on my iPhone to create a WiFi network with a weak password which is just "password" and i will be connecting my old iphone 6 to it as a dummy user.

### Network Security
Personal WiFi networks can have different kinds of security and authentication mechanisms and which ones it has will change the way you attack it. The different security protocols are wired equivalent privacy (WEP), wifi protected setup (WPS), WiFi protected access (WPA/WPA2-PSK), and recently WPA3. WEP is very old, insecure and not really used anymore so i wont be covering it but heres a [link](https://null-byte.wonderhowto.com/how-to/hack-wi-fi-hunting-down-cracking-wep-networks-0183712/) with more information if you are curious. WPA3 is very new and i havent seen any routers with it yet although im sure they are out there but airgeddon has no attacks for it so i also wont be covering it. Most networks you will come across will be protected by WPA/WPA2 and possibly WPS and these are the protocols i will focus on.

#### WPA/WPA2
This is the type of security you will mostly see in personal networks and is one of the ones we will be attacking. WPA2 came out in 2004 and is an upgrade on the origional WPA that came out in 2003. WPA is an encryption system that allows people to authenticate themselves to a router and then also encrypt each packet flowing to and from the router.
##### Handshake attack
The weakness airgeddon exploits lies in the authentication/encryption protocol called WPA2 pre-shared key (PSK). The protocol uses something called the 4-way handshake to authenticate the user and exchange the information it and the user need to create a shared secret so traffic can be encrypted. Here because of the wireless nature of communications when a user connects to the router for the first time if we put our wifi adapter in monitor mode and just get it to capture all the traffic flowing to the router we can try to capture all the information given away during the unencrypted 4-way handshake. With this information we have everything we need to try to recover the password offline using a dictionary attack or bruteforce. However users dont often have to enter the password to their WiFi network so to force this situation to happen we can flood the router with deauthentication packets to kick users off and since most devices are set up to reconnect automatically this will force the 4-way handshake to happen when we want it to. Below is a diagram of the 4-way handshake and the information that is transmitted by each party.
                    
```seq
Router->User: Router_Nonce
Note Right of User: User creates the PTK (Pairwise transient key), and sends it's nonce to router so it can create the same PTK
User-->Router: User_Nonce + MIC (message integrity code) which includes the authentication
Note Left of Router: Router creates PTK, and verifies it is the same as user by using MIC (indicates correct password)
Router->User: Sends GTK (group temporal key) (encrypted by PTK) + sequence number + MIC
User->Router: User Acknowleges keys installed
```
The pre-shared key is just the hashed WiFi password salted with the SSID and is never sent over the air but rather is just calculated by the router and user individually since they both have everything you need (password and SSID).

**PSK = Hash_Function(Wi-Fi password + Wi-Fi SSID, Length of SSID + 4096 iterations of SHA1)**

The PSK is not used to encrypt traffic but it is used to derive the pairwise transit key (PTK) which is then used to encrypt all data flowing between the user and the router.

**PTK = PSK + Router_nonce + User_nonce + MAC (router) and MAC (user)**

Now while all of this stuff is happening during the 4-way handshake we are secretly capturing all of it. Now we have the user_nonce, router_nonce, router MAC and user MAC, All we need is the PSK which we cant get without the password. However what we can do is guess a password and create our own PTK then use the MIC to verify if we got it right. So now we will bootstrap that and just try a ton of different passwords until we get the right one. The best part is we dont even need the router now this can be done entirely offline.

#### WPS
Wifi protected setup is another layer added to routers that was meant to ease authentication by using a button or a small PIN. The WPS attacks section of airgeddon is number 8 on the main menu. From there there are 5 kinds of attacks.
##### Custom PIN association
This attack lets you recover the WPA-PSK password if you know the WPS PIN
##### Pixie Dust Attack
This attack is made possible by manufacturers that used bad random number generators to create secret nonces. Knowing the two non-random nonces the attack is able to recover the WPS PIN within a couple of minutes.
##### Brute Force
WPS has a PIN that cannot be changed and is mated to the router forever. There are several problems with this for one the PINs were eight digit numbers and that made them vounerable to bruteforcing. The last digit of the PIN was a checksum so 10^7=10,000,000 possible combinations which could be bruteforced in less than a day. However, most router manufactures cought onto this and made most have a timeout after an incorrect pin is guessed which makes this attack not fesible.
##### Known PINs database attack
Some manufactures used security through obscurity and just assumed people wouldnt figure out what algorithm they used to calculate the pins (which are derived from the router MAC address). They were wrong...so a certain list of routers can be accessed by calculating the pin from the MAC address
##### Null PIN attack
This one if more rare to find but some really bad WPS router implementations allowed the null pin to connect

### Evil Twin Attack With captive portal
Another great way to hack into a wifi network is to trick the user into giving you the password. This avoids all the messy brute forcing stuff that is done in the handshake attack. An evil twin attack with captive portal involves you doing the same process to initially capture the 4-way handshake and essentially uses this information plus the network information to copy the WiFi network and create an unsecured twin access point of your own. Now the one thing with this attack is you must be very close to the router because once youve created your twin you will flood the users with deauth packets on the other router and hope that they will see your access point with the same name but better signal and try to connect. Once they are connected and they try to access the internet one of those portals that u often see in free WiFi areas that ask you to accept terms to connect will pop up. The only difference here is it will ask for a password. This attack relies on the fact that the user gets annoyed with their WiFi constantly dropping and just enters the password to get it working again. This attack can also be made more sucessful by changing the look of the portal. For example if they use rogers internet you could make it look like the rogers login page to convince them its legit. Once they enter the password it will be checked against the 4-way handshake and if it is correct, congratulations you got the password and it will be revealed to you in plain letters.

## 4-Way Handshake Demo
1. From the main menu type 2 to put network card in monitor mode so we can search for targets
2. type 5 for handshake/PMKID tools menu
3. type 4 to explore for tragets this shouldn't take too long and once u think you've gathered enough intel you can press ctrl+c to end the search
	* Press enter and it will start to scan for WiFi networks that are protected by WPA/WPA2
	* You will see several things on the screen that just poped up first youll see a collumn that says PWR that number the closer it is to 0 the closer the WiFi network is to you
	* You will also see a data column this is important because it shows you that data is being sent from client devices to the router and we need a client to be connected so we can deauthenticate them in the next step to capture the 4-way handshake. However a client can be connected without sending data so its not a must
	* The CH column stands for channel which is the WiFi channel it is broadcasting on. This will tell you if it is a 2.4GHz Wifi network (channels 1-11) or a 5Ghz wifi network (channels 36-165)
	* The AUTH is also important as we are looking for channels that have PSK authentication
4. The next screen is to select the target now the ones highlighted in orage with an asterix next to the number are the ones you want. The colour indicates someone is useing the network so we can deauthenticate them. Youll also only want to pick networks with a name since the PSK is salted with the SSID we need it to do the decryption later so this wont work for hidden networks. Im going to type the number for iphone network since that is our test network.
5. Now we are going to type 6 to attempt to capture the handshake
6. These are just different ways to capture the handshake option 2 deauth aureplay is fine but if it doesnt work you can try the others
	* for the timeout the default of 20 seconds is fine
	* you can see on the iphone screen that it has been kicked off the wifi and it will try to reconnect automatically
	* success the handshake has been captured save it to wherever you want

7. Now we are going to type 0 to return to the main menu so we can get to the bruteforce tools
8. Type 6 for offline WPA/WPA2 decrypt menu
9. Type 1 since this is a personal network
10. Now there are a lot of different options here but since we are in a VM we dont have access to the GPU so we are going to select 1 (aircrack) Dictionary attack against Handshake/PMKID capture file
	* It will ask if you want to use the handshake file you captured this session type yes
	* Also yes we want to use the same network we have already selected
	* Now enter the path of the dictionary file you are going to use. I use rockyou which comes with kali linux it has about 14 million common passwords but i would also reccomend filtering out duplicates as there are a lot and that will just waste time.
	* now start the decription and what it is going to do is check each password in the list against the captured 4-way handshake and if one matches weve got the password
	* Success we found the password (which is password) very quickly probabaly because its so common and was near the top of the list
	* now you can save the trophy file if you want and we are all done we powned the network and you can log onto the wifi with the password you found
