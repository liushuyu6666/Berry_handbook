# Ubuntu Server
follow this [link](https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#1-overview), which will tell you:

## prepare your SD card
1.  instal rpi-imager on Linux and then you can select one system from the list.
2.  burn Ubuntu server to your SD card.
3.  network configuration.

## ssh connnection
1. insert the SD card and power it
2. when the green light finish flashing, which always takes 3 to 4 minutes, unplugin the power cable
3. plugin again
4. to get your raspberry pi's ip address, it is better to check your modem admin page, but you can use `nmap -sP 192.168.2.0/24` or `arp -an | grep e4:5f:1` to find the raspberry pi, the `e4:5f:1` is the MAC address
5. after finding the ip address, you can ping the ip address
6. but before that, make sure that you enable the raspberry pi's ssh. use `ssh ubuntu@<your_raspberry_pi_ip_address>` to ssh it
7. 

## get MAC address
1. goto `/sys/class/net`
2. select the net interface that you want to check, normally it should be `wlan0`
3. check `vi address` there

## change hostname
1. goto `/etc/hostname`
2. goto `/etc/hosts` add one line `127.0.0.1 newHostName`
3. restart
4. check the status by `hostnamectl`

## get hostname of adjacent nodes
1. you can configure adjacent nodes ip address in `/etc/hosts`

# raspbian

almost the same as Ubuntu server, slight difference are listed below:

## enable ssh
1. create an empty file named `ssh` without any extension in the `boot` directory