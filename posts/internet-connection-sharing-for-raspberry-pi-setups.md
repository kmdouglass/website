<!--
.. title: Internet Connection Sharing for Raspberry Pi Setups
.. slug: internet-connection-sharing-for-raspberry-pi-setups
.. date: 2024-09-13 15:26:11 UTC+02:00
.. tags: raspberry pi, networking
.. category: homelab
.. link: 
.. description: 
.. type: text
-->

Today I decided to set up an old Raspberry Pi 3B+ for a task in the lab. After burning the latest Raspberry Pi OS Lite image on the SD card, I booted it up and was faced with the unfortunately common problem of network access. It would have taken days to get IT to register the Pi's MAC address on our system, and I did not want to wait that long.

Luckily, I had a spare network crossover cable and an extra ethernet interface on my Windows work laptop, so I plugged the Pi directly into the laptop and enabled Microsoft Internet Connection Sharing (ICS) between the network connection through which I was connected to the internet and the connection to the Pi. In my specific example:

1. Press the Windows key and navigate to `View network connections`
2. Right click on my internet connection (`Ethernet 2` in my case), select `Properties...`, and then the `Sharing` tab.
3. Check `Allow other network users to connect...` and in the `Home networking connection:` dropdown, select the connection corresponding to the Pi (`Ethernet` in my case).
4. Check `Allow other network users to control...`. I'm not sure whether this is necessary.

Click OK and restart the Pi if it's already connected. Once it restarts, it should now have internet access through the laptop.

Next I wanted to connect with SSH to the Pi from my laptop and I needed to know the Pi's IP address. Luckily, ICS uses the `mshome.net` domain name for the network, and the Raspberry Pi by default has the hostname `raspberrypi`. So getting the IP is as easy running the `ping raspberrypi.mshome.net` command in Powershell.
