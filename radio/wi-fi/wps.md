# WPS

## Theory

Wi-Fi Protected Setup (WPS) is a simplified configuration protocol designed to make connecting devices to a secure wireless network easier.
WPS employs an 8-digit PIN for user network connection, verifying the first 4 digits before proceeding to check the remaining 4. This allows for a potential Brute-Force attack on the first set of digits followed by the second set, with a total of only 11,000 possible combinations.

There are three main methods for setting up a connection using WPS :

* **Push Button Configuration (PBC) :** Press the WPS button on the router and then activate the WPS function on the device to connect. The two devices will automatically establish a connection.

* **PIN-based Configuration :** Each WPS-enabled device has a unique PIN code. It can be entered on the router through its web interface, or vice versa, to establish the connection.

* **Near-field communication Configuration :** In which the user has to bring the new client close to the access point to allow a near field communication between the devices. NFC Forum–compliant RFID tags can also be used.

## Attacks

Here are some known attacks on Wi-Fi Protected Setup (WPS) :

* **PIN Brute-Force Attack :** Some routers with WPS allow users to connect by entering an 8-digit PIN code. A brute-force attack involves trying all possible combinations until the correct PIN is found.

* **Pixie Dust Attack :** This attack exploits a weakness in the generation of WPS encryption keys. It involves exploiting vulnerabilities in certain routers during the creation of WPA/WPA2 encryption keys.

* **PIN Revelation Attack :** Certain WPS-enabled routers may reveal information about the validity of the PIN during a connection attempt. This information disclosure can aid an attacker in narrowing down the search space during a brute-force attack.

* **Access Point Enumeration Attack :** Some routers with WPS may be vulnerable to an attack that allows an attacker to determine which access points are present in a given area.

## Practice

### Recon

Wash is a tool used to detect access points with WPS (Wi-Fi Protected Setup) enabled. It can perform a survey either directly from a live interface or by scanning a list of pcap files. As an auxiliary utility, Wash is specifically crafted to showcase WPS-enabled Access Points along with their primary characteristics. This tool is bundled within the Reaver package.
```bash
wash -i [INTERFACE]
```

### Brute-Force Attack

In December 2011, researcher [Stefan Viehböck](https://twitter.com/sviehb) identified a design and implementation flaw that renders PIN-based WPS vulnerable to brute-force attacks on WPS-enabled Wi-Fi networks. A successful exploitation of this flaw grants unauthorized individuals access to the network, and the sole effective solution is to disable WPS.

Today, two primary tools are available for executing this operation: [Reaver](https://github.com/t6x/reaver-wps-fork-t6x) and [Bully](https://github.com/aanarchyy/bully). 
Reaver is crafted to be a robust and effective attack method targeting WPS. It has undergone testing against a diverse range of access points and WPS implementations. On the other hand, Bully represents a fresh implementation of the WPS brute force attack, coded in C. It offers several advantages over the original Reaver code, including fewer dependencies, enhanced memory and CPU performance, accurate handling of endianness, and a more resilient set of options.

```bash
# /!\ Beware: change placeholder values INTERFACE, BSSID-MAC, CHANNEL

# With reaver :
reaver -i [INTERFACE] -b [BSSID] -c [CHANNEL] -b -f -N [-L -d 2] -vvroot   

# With bully :
bully [INTERFACE] -b [BSSID] -c [CHANNEL] -S -F -B -v 3
```

### Pixie Dust attack

During the summer of 2014, [Dominique Bongard](https://twitter.com/Reversity) identified a security vulnerability he named the Pixie Dust attack. This exploit specifically targets the default WPS implementation found in wireless chips produced by various manufacturers, including Ralink, MediaTek, Realtek, and Broadcom. The attack exploits a deficiency in randomization during the generation of the E-S1 and E-S2 "secret" nonces. With knowledge of these nonces, the PIN can be retrieved in a matter of minutes. To streamline the process, a tool called pixiewps was developed, and a new version of Reaver was created to automate the attack.

Check this [list](https://docs.google.com/spreadsheets/d/1tSlbqVQ59kGn8hgmwcPTHUECQ3o9YhXR91A_p7Nnj5Y) to know which router model is vulnerable.


```bash
# /!\ Beware: change placeholder values INTERFACE, BSSID-MAC, CHANNEL

# With reaver :
reaver -i [INTERFACE] -b [BSSID] -c [CHANNEL] -K 1 -N -vv

# With bully :
bully  [INTERFACE] -b [BSSID] -d -v 3
```

### Null Pin attack

Certain poorly implemented systems permitted the use of a Null PIN for connections, which is highly unusual. Reaver has the capability to test for this, whereas Bully lacks this specific functionality.

```bash
# /!\ Beware: change placeholder values INTERFACE, BSSID-MAC, CHANNEL

reaver -i [INTERFACE] -b [BSSID] -c [CHANNEL] -f -N -g 1 -vv -p ''
```

## All-in-one tools

All the mentioned WPS attacks can be effortlessly executed using airgeddon or wifite2.

Airgeddon : https://github.com/v1s1t0r1sh3r3/airgeddon

Wifite2 : https://github.com/derv82/wifite2

## Resources

Reaver : https://github.com/t6x/reaver-wps-fork-t6x

Bully : https://github.com/aanarchyy/bully

Airgeddon : https://github.com/v1s1t0r1sh3r3/airgeddon

Wifite2 : https://github.com/derv82/wifite2

Vulnerable router models : https://docs.google.com/spreadsheets/d/1tSlbqVQ59kGn8hgmwcPTHUECQ3o9YhXR91A_p7Nnj5Y
