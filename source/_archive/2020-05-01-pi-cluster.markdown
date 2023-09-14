---
layout: post
title: "PI Cluster"
date: 2019-01-24 12:00
comments: true
categories: linux
---

{% img right /images/posts/pi-cluster.jpg 322 372 Pi Cluster %}

### Hardware
- 4x Raspberry Pi 3 B+
- 4x RJ45 Cat6 ethernet cable
- 4x 32GB Micro SDHC
- 4x Micro USB cable
- 1x Switch TP-LINK TL-SF1005D 5-Port 10/100Mbps
- 5x Anker USB Cables
- 1x Anker Port Wall Chargers
- 1x Soldering Iron
- 1x Hot Glue Gun

### Requirements
- unix system (linux or mac)
- flash v2.2.0 installation
- hypriot OS v1.9.0 download
- [Pi Image Download](https://www.raspberrypi.org/documentation/installation/installing-images/)

### SSH keys
We need a SSH key in order to connect to the cluster without having to type the password every time we access.

In case you don't have any, run this command and follow the steps.

`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

`Add the key to your ssh agent, assuming our keys generated are id_rsa and id_rsa.pub

`ssh-add ~/.ssh/id_rsa`

You can find a more in depth explanation in this tutorial






###  Sync Script

some random description, lorem ipsum...

{% codeblock clusteri-sync.sh lang:bash %}
#pi@raspberry

this=ifconfig | grep -Eo 'inet (addr:)?([0-9]*\.){3}[0-9]*' | grep -Eo '([0-9]*\.){3}[0-9]*' | grep -v '127.0.0.1'

declare NODES=("localhost
192.168.0.1
192.168.0.2
192.168.0.3
192.168.0.4
")

SSID="Test Wifi Network"
PSK="SecretPassword"

echo "network={
   ssid=\"$SSID\"
   psk=\"$PSKd\"
}" >> /etc/wpa_supplicant/wpa_supplicant.conf

sudo systemctl enable ssh
sudo systemctl start ssh

sudo apt-get install nginx

sudo shutdown -r now

sudo chown -R www-data:www-data srv/



su www-data

for $NODE in $NODES
do
    rsync -v -r --delete -e ssh /var/www/ "www-data@{$NODE}:/var/www/"
done
{% endcodeblock %}

