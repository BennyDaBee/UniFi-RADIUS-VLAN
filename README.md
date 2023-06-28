# UniFi-RADIUS-VLAN
A documentation of how to setup UniFi wireless with Windows Server using NPS to have MAC based VLAN Assignment on one SSID. 

## Prerequisites
UniFi Controller<br>
UniFi AP(s)<br>
Windows Server 2012/2019/2022 with NPS. This works best with Active Directory Enabled, but not a requirement<br><br>
Password policy on the server must be similar to the following:<br>
Password Length: <12<br>
Complexity: Disabled<br>

## Setting up your NPS and RADIUS
On your Windows Server, go to either local accounts or Active Directory Users and computers. <br>
You will want to create a group/security role for the devices. i.e. UNIFI-RADIUS-VLAN10 <br>
![image](https://github.com/BennyDaBee/UniFi-RADIUS-VLAN/assets/97147515/95ec3f27-b61a-4935-be0a-c3d8fd6bbff8) <br>
Next you will want to create a user account. For the username and password, you will want to set it to the MAC address of the device. Then assign it thr group that you created. <br>
![image](https://github.com/BennyDaBee/UniFi-RADIUS-VLAN/assets/97147515/c4e473d1-4f0d-4b46-8dff-e472facb3549) <br>
I have had issue with windows firewall blocking RADIUS even with the port allowed, so I did allow all traffic from the AP(s) IPs on the Inbound Traffic rules. <br>
New Inbound rule, Custom Rule Type, All Program, all ports and protocols, Scope remote IP These IP Address. Add in your AP Ips. Action Allow Profile Private/Domain, Name whatever you want it.<br>
![image](https://github.com/BennyDaBee/UniFi-RADIUS-VLAN/assets/97147515/ff790056-ee8c-412e-b2cd-3c2624f78862) <br>
Open Network Policy Server<br>
Under RADIUS Client and Servers, right click RADIUS CLients and click new. Add the IP of your AP, and create a secret key that we will need later. Do this for each AP. Use the same secret for each AP<br>
![image](https://github.com/BennyDaBee/UniFi-RADIUS-VLAN/assets/97147515/3fa37bef-216a-4f29-b177-429d42760c8c) <br>
Next go to Policies, right click Network Policies and click new. <br>
Give it a name i.e. Wireless-RADIUS-VLAN10. Type of Newrok Access leave at unspecified<br>
![image](https://github.com/BennyDaBee/UniFi-RADIUS-VLAN/assets/97147515/87a5f4f5-06bf-43c9-a127-dc8c84615bba) <br>
Hit next, adn then Next click add, then Windows Groups. Click Add Groups, then type in the name of the group you made, then click OK, then OK<br>
![image](https://github.com/BennyDaBee/UniFi-RADIUS-VLAN/assets/97147515/03d296c7-1ac7-4d6e-aa3f-9dbdf92a783a) <br>
Click next, access granted, click next. <br>
On the EAP Types Screen, click Add, then click PEAP. Under less secure, I was having issues, so I do have all methods except the last checked until I can test further. <br>
![image](https://github.com/BennyDaBee/UniFi-RADIUS-VLAN/assets/97147515/ca4c7b60-d0a6-45b7-9c0f-c7d2ee8f1a66) <br>
Click Next, and on the Constraints click next.<br>
On the Settings screen, Under Standard it shoudl match the following screenshot. Change the Tunnel-Pvt-Group-ID to the VLAN that you want to assign the device/group to.  <br>
![image](https://github.com/BennyDaBee/UniFi-RADIUS-VLAN/assets/97147515/abc57d59-a12e-4b64-91ae-8b2374897006) <br>
Under the encryption, uncheck no encryption, click next, and then click finish. This is all for the Windows Server and NPS Side<br>
### NOTE: YOU WILL HAVE TO CREATE A NEW POLICY FOR EACH VLAN <br>

## Setting up UniFi 
In your unifi controller onthe site you want to set this up on, go to settings, and then profiles. <br>
Under RADIUS, click Create New Radius Profile <br>
Profile Name can be somthing simeple i.e. Windows RADIUS Server <br>
Check the "Enable RADIUS Assigned VLAN for Wireless Network" <br>
The IP should be the IP of your Windows Server and the port should be 1812. The Password will be the secret we created above. <br>
I personally enable accounting. <br>
Enable Interim Update, leave the interval as default. <br>
The Accounting server if you eanble it, will be the same as the Auth Server, except port 1813. Make sure to use the same secret. <br>
![image](https://github.com/BennyDaBee/UniFi-RADIUS-VLAN/assets/97147515/39eccf4d-4b98-424d-a6d1-a400016bb5b2) <br>
Next go to Wireless Networks and Create New Wireless Network <br>
Make the SSID whatever you please and apply password if you want to. <br> 
Click Advanced Options, scrol all the way down to RADIUS MAC AUTHENTICATION, expand it. Click Enable. Unde the RADIUS Profile, click the one we just made. <br>
Under MAC Adddress Format, choose aabbccddeeff. <br>
![image](https://github.com/BennyDaBee/UniFi-RADIUS-VLAN/assets/97147515/24635ef5-c4ae-4c98-984e-e1ff5ef4d132) <br>

You are all set and should now have MAC Based VLAN








