<p align="center">
Next, I went into the Domain Controller VM: <br/><br />
<img src="https://i.imgur.com/58rEzxr.png" alt="Homelab Steps">
<br />
<br />
<br />
Once logged in, the VM automatically opens Server Manager. Here I want to install Active Directory. I started the prcoess by clicking add roles and features: <br/><br />
<img src="https://i.imgur.com/VGNRKLW.png" alt="Homelab Steps">
<br />
<br />
<br />
Then I clicked next until it asked what I wanted to install. I selected Active Directory Domain Services: <br/><br />
<img src="https://i.imgur.com/ixR6S7W.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I clicked through until it let me install. Once installation was done, I clicked the flag in the top right to promote the server to a domain controller: <br/><br />
<img src="https://i.imgur.com/WzmNrB5.png" alt="Homelab Steps">
<br />
<br />
<br />
First I created a new forest and named it Dooley.local: <br/><br />
<img src="https://i.imgur.com/QvX6HXJ.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I went ahead and set the password: <br/><br />
<img src="https://i.imgur.com/XgjxMSi.png" alt="Homelab Steps">
<br />
<br />
<br />
Finally, I clicked install and the server began the process of being promoted to a domain controller: <br/><br />
<img src="https://i.imgur.com/xaXG76S.png" alt="Homelab Steps">
<br />
<br />
<br />
After the machine rebooted, I was able to access all related Active Directory tools, such as the Administrative Center: <br/><br />
<img src="https://i.imgur.com/7a1zrnC.png" alt="Homelab Steps">
<br />
<br />
<br />
Next I wanted to create a bunch of groups, users, and organizational units. First I navigated to Active Directory Users and Computers and went into Dooley.local. For now I just created everything in the Users folder. I started with the groups by right clicking and selecting group: <br/><br />
<img src="https://i.imgur.com/HkfOR0d.png" alt="Homelab Steps">
<br />
<br />
<br />
The settings were exactly what I wanted so all I had to do was name the group: <br/><br />
<img src="https://i.imgur.com/LCyWlNc.png" alt="Homelab Steps">
<br />
<br />
<br />
I repeated this process two other times, making three groups in total: <br/><br />
<img src="https://i.imgur.com/RGAG43f.png" alt="Homelab Steps">
<br />
<br />
<br />
Next I wanted to create the users. I did so by right clicking and selecting user. : <br/><br />
<img src="https://i.imgur.com/b0FiPkY.png" alt="Homelab Steps">
<br />
<br />
<br />
The first step was setting the user name and logon name: <br/><br />
<img src="https://i.imgur.com/ZW0OBqa.png" alt="Homelab Steps">
<br />
<br />
<br />
The second step was to set a password: <br/><br />
<img src="https://i.imgur.com/ch2oRA7.png" alt="Homelab Steps">
<br />
<br />
<br />
The final step was to confirm everything and finish making the user: <br/><br />
<img src="https://i.imgur.com/zZ4itN4.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
Next I wanted assign each user their respective group. I did so by double clicking the group of choice, clicking members, clicking add, typing their name in the search box, clicking check names, and then clicking okay: <br/><br />
<img src="https://i.imgur.com/nFhHkxf.png" alt="Homelab Steps">
<br />
<br />
<br />
Here is how it looked once each user had been assigned a group: <br/><br />
<img src="https://i.imgur.com/IgJQaO5.png" alt="Homelab Steps">
<br />
<br />
<br />
Next I wanted to create the organizational units. I started by right clicking and selecting new organizational unit: <br/><br />
<img src="https://i.imgur.com/KSudUPy.png" alt="Homelab Steps">
<br />
<br />
<br />
I went ahead and made three units. Then I moved each user and group into their respective OU: <br/><br />
<img src="https://i.imgur.com/jTDvnvH.png" alt="Homelab Steps">
<br />
<br />
<br />
After all that, I needed to make a change in the Client VM so I went ahead and jumped into the Client: <br/><br />
<img src="https://i.imgur.com/s37qx1g.png" alt="Homelab Steps">
<br />
<br />
<br />
First I needed to specify the DNS server IP within Ethernet Instance 0.2. This allows the client to refer to the DC as the DNS server. This was similar to when I fixed the Private IP issue previously: <br/><br />
<img src="https://i.imgur.com/jMvT0nj.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I when into the about section in the settings, clicked rename this pc (advanced), clicked change, then set it to be a member of the DOOLEY domain: <br/><br />
<img src="https://i.imgur.com/GSp5ZC0.png" alt="Homelab Steps">
<br />
<br />
<br />
I was then prompted to login with the DC credentials: <br/><br />
<img src="https://i.imgur.com/MWKfXZQ.png" alt="Homelab Steps">
<br />
<br />
<br />
Finally, I was shown a prompt confirming the Client had been connected to the Dooley Domain: <br/><br />
<img src="https://i.imgur.com/eSxhYxa.png" alt="Homelab Steps">
<br />
<br />
<br />
After logging out, I was able to log back in using one of the users I had previously created: <br/><br />
<img src="https://i.imgur.com/coSpXxv.png" alt="Homelab Steps">
<br />
<br />
<br />
The last thing I wanted to do was enable RDP for my user (rdooley) into the Client VM, as it will be important later. I did this by going into the remote desktop section of the settings and adding my username to the list of allowed users for RDP: <br/><br />
<img src="https://i.imgur.com/7xdEgnh.png" alt="Homelab Steps">
<br />
<br />
<br />
This was all for setting up Active Directory. The project documentation is continued in the Network Drive Steps: <br/><br />
https://github.com/rdooley2/Homelab/blob/main/NetworkDrive.md
