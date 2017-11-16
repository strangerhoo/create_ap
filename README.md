## Features
* Create an AP (Access Point) at any channel.
* Choose one of the following encryptions: WPA, WPA2, WPA/WPA2, Open (no encryption).
* Hide your SSID.
* Disable communication between clients (client isolation).
* IEEE 802.11n & 802.11ac support
* Internet sharing methods: NATed or Bridged or None (no Internet sharing).
* Choose the AP Gateway IP (only for 'NATed' and 'None' Internet sharing methods).
* You can create an AP with the same interface you are getting your Internet connection.
* You can pass your SSID and password through pipe or through arguments (see examples).


## Dependencies
### General
* bash (to run this script)
* util-linux (for getopt)
* procps or procps-ng
* hostapd
* iproute2
* iw
* iwconfig (you only need this if 'iw' can not recognize your adapter)
* haveged (optional)

### For 'NATed' or 'None' Internet sharing method
* dnsmasq
* iptables


## Installation
### Generic
    git clone https://github.com/oblique/create_ap
    cd create_ap
    make install

### ArchLinux
    pacman -S create_ap

### Gentoo
    emerge layman
    layman -f -a jorgicio
    emerge net-wireless/create_ap

## Examples
### No passphrase (open network):
    create_ap wlan0 eth0 MyAccessPoint

### WPA + WPA2 passphrase:
    create_ap wlan0 eth0 MyAccessPoint MyPassPhrase

### AP without Internet sharing:
    create_ap -n wlan0 MyAccessPoint MyPassPhrase

### Bridged Internet sharing:
    create_ap -m bridge wlan0 eth0 MyAccessPoint MyPassPhrase

### Bridged Internet sharing (pre-configured bridge interface):
    create_ap -m bridge wlan0 br0 MyAccessPoint MyPassPhrase

### Internet sharing from the same WiFi interface:
    create_ap wlan0 wlan0 MyAccessPoint MyPassPhrase

### Choose a different WiFi adapter driver
    create_ap --driver rtl871xdrv wlan0 eth0 MyAccessPoint MyPassPhrase

### No passphrase (open network) using pipe:
    echo -e "MyAccessPoint" | create_ap wlan0 eth0

### WPA + WPA2 passphrase using pipe:
    echo -e "MyAccessPoint\nMyPassPhrase" | create_ap wlan0 eth0

### Enable IEEE 802.11n
    create_ap --ieee80211n --ht_capab '[HT40+]' wlan0 eth0 MyAccessPoint MyPassPhrase

### Client Isolation:
    create_ap --isolate-clients wlan0 eth0 MyAccessPoint MyPassPhrase

## Systemd service
Using the persistent [systemd](https://wiki.archlinux.org/index.php/systemd#Basic_systemctl_usage) service
### Start service immediately:
    systemctl start create_ap

### Start on boot:
    systemctl enable create_ap


## License
FreeBSD
====================================================
WiFi repeater with a single WiFi adapter in Debian


up vote
2
down vote
favorite
1
Is it possible to create a WiFi repeater with a single WiFi adapter in Debian , to increase the range of my WiFi network?

debian wifi
shareimprove this question
edited Jan 28 at 16:41

GAD3R
15.2k103174
asked Jan 28 at 16:15

Nathan
135
  	 	
+GAD3R Sorry I'm not sure what you would call it. Maybe a WiFi booster? Something which connects to a WiFi network and acts as a second access point to that network. Essentially extending the range of the WiFi network. – Nathan Jan 28 at 16:33
  	 	
e,g: create a WiFi hotspot from the same wifi card ? can be helpful ? – GAD3R Jan 28 at 16:35
1	 	
Yes, connect to a WiFi network and create a hotspot on the same WiFi adapter. – Nathan Jan 28 at 16:37
1	 	
As a side-note, a wifi adapter can only use on a single channel at a time, which means the AP will use the same frequency on both sides, thus cause interference with its source. It may not be that bad but it will surely lower the maximum wifi bandwidth. – Julie Pelletier Jan 28 at 17:50
  	 	
@JuliePelletier Interesting, thanks for the info. – Nathan Jan 28 at 18:15
add a comment
1 Answer
active oldest votes
up vote
3
down vote
accepted
To increase the range of your WiFi network, you can create an access point from the same wifi card.

Install the required packages

apt-get install build-essential git
Install create_ap:

git clone https://github.com/oblique/create_ap
cd create_ap
make install
Start the service and enable it :

systemctl start create_ap
systemctl enable create_ap
To create an AP :

Internet sharing from the same WiFi interface:
create_ap wlan0 wlan0 MyAccessPoint MyPassPhrase
