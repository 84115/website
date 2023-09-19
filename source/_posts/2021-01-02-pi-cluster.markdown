---
layout: post
title: "PI Cluster"
date: 2021-01-02 23:17
comments: false
categories: linux pi
---

> As of writing this is simply a hobby project at the moment and doesn't dive into server configuration.

{% img right /images/posts/pi-cluster.jpg 322 372 Pi Cluster %}

### Hardware
- 4x [Raspberry Pi 3 B+](https://www.amazon.co.uk/Raspberry-Pi-3-Model-B/dp/B07BDR5PDW)
- 4x [RJ45 Cat6 ethernet cable](https://www.amazon.co.uk/rhinocables-Ethernet-Network-Gigabit-Internet/dp/B01MY79D10)
- 4x 32GB micro SDHC
- 1x [Switch TP-LINK TL-SF1005D 5-Port 10/100Mbps](https://www.tp-link.com/uk/business-networking/unmanaged-switch/tl-sf1005d)
- 4x USB A to Micro-B
- 1x Anker Port Wall Charger
- 1x [USB A to 2.5mm DC 5V Barrel](https://www.amazon.co.uk/JSER-2-5mm-Barrel-Connector-Charge/dp/B00YAOJTTA/ref=sr_1_2_sspa?crid=3FLJCWCECDAYB)

### Requirements
- unix system (linux or mac)
- [Pi Image Download](https://www.raspberrypi.org/documentation/installation/installing-images)

### Install Pi Images
Follow this [tutorial](https://www.raspberrypi.com/documentation/computers/getting-started.html#installing-the-operating-system) from Raspberry Pi to install your images to each of the SD cards.

### SSH keys
We need a SSH key in order to connect to the cluster without having to type the password every time we access.

In case you don't have any, run this command and follow the steps.

`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

`Add the key to your ssh agent, assuming our keys generated are id_rsa and id_rsa.pub

`ssh-add ~/.ssh/id_rsa`

The enable `ssh` if it isn't running:

```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

###  Wifi Setup Script

Next we'll need to set up Wifi on the Master Node only:

```bash
SSID="YOUR-WIFI-SSID"
PSK="YOUR-WIFI-PASSWORD"

echo "network={
   ssid=\"$SSID\"
   psk=\"$PSK\"
}" >> /etc/wpa_supplicant/wpa_supplicant.conf

sudo shutdown -r now
```

### Running Commands On Multiple Instances
This can be done by iterating of the `NODES` list (modify as required) on the Master Node.
Then you can add the required command calls in this mock example, I'll clear all the data in the `/var/www/` directory:

```bash
declare NODES=("localhost
192.168.0.1
192.168.0.2
192.168.0.3
192.168.0.4
")

for $NODE in $NODES
do
    rsync -v -r --delete -e ssh /var/www/ "www-data@{$NODE}:/var/www/"
done
```

### In Conclusion
This is just the foundation for setting up pi cluster hardware without diving into server configuration as this is just a hobby project at the moment.
I may research [Kubernetes](https://kubernetes.io) in the future, of which I may create a follow up article on.
