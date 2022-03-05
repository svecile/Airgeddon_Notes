# Airgeddon
Airgeddon is a penatration testing tool made to perform a variety of attacks against wireless access points. These attacks can be used by penatration testers to audit the security of wireless access points or networks. Airgeddon is a bash script that brings together many standalone hacking tools that you may or may not be aware of into one convienient interface.

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
	- For large netowkrs such as our university WiFi uwosecure-v2
	- Create evil twin access point to capture enterprise credentials
	- Smooth and noisey modes to evade detection or to just hammer the network
- WEP all-in-one attack
	- WEP is a very outdated, very vounerable network security protocol and is not widely used anymore and with limited time i'd rather focus on the more current aspects of airgeddon
	- If you would like to know more about WEP and WEP cracking [here's](https://null-byte.wonderhowto.com/how-to/hack-wi-fi-hunting-down-cracking-wep-networks-0183712/) an article from Null Byte Wonder How To, that uses aircrack-ng

So, wether your auditing your home WiFi or large enterprise networks airgeddon has the capability to fit your needs. In this tutorial I will focus on personal network WPA/WPA2 handshake capture, password decription and Evil Twin attacks because I think they are the most relivant to you, I am able to set up a personal network for a demo and i think they are just fun. I will breafly touch on Enterprise attacks but since i dont have an enterprise network to test on there will be no demo (the fun part). 

## So How Do We Go About Hacking WiFi networks?
### Equiptement
#### Software
I will be using a [VirtualBox](https://www.virtualbox.org/) version of [Kali Linux](https://www.kali.org/) which can be found [here](https://www.kali.org/get-kali/#kali-virtual-machines). You will also need to [install airgeddon](https://github.com/v1s1t0r1sh3r3/airgeddon/wiki/Installation%20&%20Usage#generic-installation) onto kali which can be done by entering three simple commands into the terminal.
`1. git clone https://github.com/v1s1t0r1sh3r3/airgeddon.git`
`2. cd airgeddon`
`3. sudo bash airgeddon.sh`
Once the third command is run the airgeddon script will start and check if you have all the nessicary packages installed and if you dont it will ask if you want it to install them for you, to which you should say yes.

Airgeddon can also be used on other operating systems such as Windows or Mac OSX using docker and you can find instructions for that setup [here](https://github.com/v1s1t0r1sh3r3/airgeddon/wiki/Docker). It can also be used in much the same way as with kali on other [compadable Linux distrobutions](https://github.com/v1s1t0r1sh3r3/airgeddon/wiki/Compatibility).

### Hardware
To run attacks against WiFi networks you need to have a WiFi adapter that can do packet injection so most likley the WiFi adapter you have in your laptop or computer won't work. Specifically its the wireless chipset within the adapter that will determine if it can do packet injection or not. A list of compadable cards and chipsets can be found [here](https://github.com/v1s1t0r1sh3r3/airgeddon/wiki/Cards%20and%20Chipsets).
I personally have the Alpha AWUS036NHA with the Atheros AR9271 chipset, and the Alpha AWUS036ACM with the MediaTek MT7612U chipset.
#### Alpha AWUS036NHA



