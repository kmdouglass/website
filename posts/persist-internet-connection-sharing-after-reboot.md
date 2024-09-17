<!--
.. title: Persist Internet Connection Sharing after Reboot
.. slug: persist-internet-connection-sharing-after-reboot
.. date: 2024-09-17 09:52:27 UTC+02:00
.. tags: raspberry pi, networking
.. category: homelab
.. link: 
.. description: 
.. type: text
-->

[In my previous post](link://post_path/posts/internet-connection-sharing-for-raspberry-pi-setups/) I wrote about how to use Microsoft's Internet Connection Sharing to share an internet connection on a Windows machine with a Raspberry Pi. Unfortunately, I learned that the ICS service settings do not persist after the Windows machine reboots, and as a result the ICS connection is lost.

The fix is explained in [this Microsoft Learn page](https://learn.microsoft.com/en-us/troubleshoot/windows-client/networking/ics-not-work-after-computer-or-service-restart).

To fix the issue, add a key in the Windows registry with the following information:


- Path: HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\SharedAccess
- Type: DWORD
- Setting: EnableRebootPersistConnection
- Value: 1

I then had to reset the shared connection by unchecking and rechecking the boxes in the `Sharing` tab of the internet connection as explained previously. After a reboot, I confirmed that I could connect to the Pi without manually re-enabling ICS.
