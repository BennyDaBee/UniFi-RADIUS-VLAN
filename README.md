# UniFi-RADIUS-VLAN
A documentation of how to setup UniFi wireless with Windows Server using NPS to have MAC based VLAN Assignment on one SSID. 

## Prerequisites
UniFi Controller<br>
UniFi AP(s)<br>
Windows Server 2012/2019/2022 with NPS. This works best with Active Directory Enabled, but not a requirement<br>
Password policy on the server must be similar to the following<br>
<br>
Expire: Never<br>
Password Length: <12<br>
Complexity: Disabled<br>

## Setting up your NPS and RADIUS
On your Windows Server, go to either local accounts or Active Directory Users and computers. 
You will want to create a group/security role for the devices. i.e. UNIFI-RADIUS-VLAN10
Next you will want to create a user account. For the username and password, you will want to set it to the MAC address of the device. 
I have had issue with windows firewall blocking RADIUS even with the port allowed, so I did allow all traffic from the AP(s) IPs on the Inbound Traffic rules.
