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
- Enterprise Network Attacks
	- Create evil twin access point to capture enterprise credentials
	- Smooth and noisey modes to evade detection or to just hammer the network
- WEP all-in-one attack
	- If you would like to know more about WEP and WEP cracking [here's](https://null-byte.wonderhowto.com/how-to/hack-wi-fi-hunting-down-cracking-wep-networks-0183712/) an article from Null Byte Wonder How To, that uses aircrack-ng

As you can see airgeddon has a ton of functionality, wether your auditing your home WiFi or large enterprise networks airgeddon has the capability to fit your needs. Unfortunatly because it has so much functionality I wont be able to go over it all in 10 minutes so i will focus on personal network WPA/WPA2 handshake capture, password decription and Evil Twin attacks because I think they are the most relivant to you, I am able to set up a personal network for a demo and i think they are just fun. I will have to skip over Enterprise attacks and since i dont have an enterprise network to test on i couldnt do a demo anyway (the fun part). WEP will also not be coveres as it is a very outdated, very vounerable network security protocol and is not widely used anymore and i'd rather focus on the more current aspects of airgeddon. WPS is still relivant so i will touch on it but not go in depth as there is too much to cover.

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
## Network Recon
Next you are going to need to do some recon and figure out the type of network you are dealing with and what kind of security it has so you can attack it. To perform wireless recon you will have to put your WiFi adapter in monitor mode which is done by typing 2 on the main airgeddon screen. There are two types of networks you will encounter personal and enterprise.

#### Enterprise Network
It will be obvious if you are dealing with an enterprise network because when you click to join the network it will ask you for a username and password. An example of this would be our university WiFi uwosecure-v2. These networks are much larger than your home network and consist of many physical and virtual networks and protocols that connect users and devices to a shared local area network (LAN). Devices on these networks are connected together using ethernet, WiFi, switches and routers so they can share data securely within the network. Airgeddon has enterprise attack functionality which is option 10 on the main menu.

#### Personal Network
This is the type of network we are going to focus on in this presentation. It is the most common type of network and is what your home WiFi would be characterised as. It requires only a password which is in contrast to a username and password combination that is needed for enterprise networks. These networks are often comprised of a router and possibly some WiFi extender pods.

For demostration pourposes in the presentation i will be using the personal hotspot on my iPhone to create a WiFi network with a weak password (password1234) and i will be connecting my old iphone 6 to it as a dummy user.

### Network Security
Personal WiFi networks can have different kinds of security and authentication mechanisms and which ones it has will change the way you attack it. The different security protocols are wired equivalent privacy (WEP), wifi protected setup (WPS), WiFi protected access (WPA/WPA2-PSK), and recently WPA3. WEP is very old, insecure and not really used anymore so i wont be covering it but heres a [link](https://null-byte.wonderhowto.com/how-to/hack-wi-fi-hunting-down-cracking-wep-networks-0183712/) with more information if you are curious. WPA3 is very new and i havent seen any routers with it yet although im sure they are out there but airgeddon has no attacks for it so i also wont be covering it. Most networks you will come across will be protected by WPA/WPA2 and possibly WPS and these are the protocols i will focus on.

#### WPA/WPA2
This is the type of security you will mostly see in personal networks and is one of the ones we will be attacking. WPA2 came out in 2004 and is an upgrade on the origional WPA that came out in 2003. 
