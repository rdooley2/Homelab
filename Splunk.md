<p align="center">
The next part of the project was to set up Splunk. First I used SSH to get into the Splunk VM. When first entering the VM I made sure to run apt update/upgrade to install new versions: <br/><br />
<img src="https://i.imgur.com/RrK6FJt.png" alt="Homelab Steps">
<br />
<br />
<br />
Once the upgrades had installed, I had the services restart: <br/><br />
<img src="https://i.imgur.com/UtubGhM.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I navigated to the Splunk website and registered for an account. I then navigated to trials & downloads and clicked on Splunk Enterprise: <br/><br />
<img src="https://i.imgur.com/gdw6mtk.png" alt="Homelab Steps">
<br />
<br />
<br />
I then grabbed the command to download the Linux .deb version: <br/><br />
<img src="https://i.imgur.com/ND0H6nl.png" alt="Homelab Steps">
<br />
<br />
<br />
Then I used SSH to get into the Splunk VM and used the command from before. I then used ls to ensure the package downloaded and used the depackage command to depackage the installer: <br/><br />
<img src="https://i.imgur.com/b1L9RwL.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I went into the /opt/splunk/bin folder and found the script that I wanted to run (splunk): <br/><br />
<img src="https://i.imgur.com/MhtwuRX.png" alt="Homelab Steps">
<br />
<br />
<br />
Then I ran the Splunk script and clicked through all of the terms and conditions: <br/><br />
<img src="https://i.imgur.com/aJ9Puje.png" alt="Homelab Steps">
<br />
<br />
<br />
At the end, I set a administrator username and password: <br/><br />
<img src="https://i.imgur.com/SXFMDdN.png" alt="Homelab Steps">
<br />
<br />
<br />
Once everything had finished running, the script gave me the URL for the Splunk interface. However, I still had one more thing to set up before I could visit it: <br/><br />
<img src="https://i.imgur.com/TWUXr81.png" alt="Homelab Steps">
<br />
<br />
<br />
I had to go in and set each port that I wanted to use as allowed for the firewall. These are the commands that I ran: <br/><br />
<pre>
ufw allow 22
ufw allow 1514
ufw allow 1515
ufw allow 8000
ufw allow 9997
</pre>
<p align="center">
<br />
When entering the Splunk IP address followed by :8000, I was able to access Splunk Enterprise. First I had to login using the username and password I had previously created: <br/><br />
<img src="https://i.imgur.com/0fZSaBc.png" alt="Homelab Steps">
<br />
<br />
<br />
Now I was brought to the dashboard. The first thing I need to do is go into preferences: <br/><br />
<img src="https://i.imgur.com/iLsiH6m.png" alt="Homelab Steps">
<br />
<br />
<br />
In the preferences I went ahead and set the time zone since that was necessary: <br/><br />
<img src="https://i.imgur.com/UJm5wXP.png" alt="Homelab Steps">
<br />
<br />
<br />
Next I wanted to install an app. I did this by first clicking on the app tab in the top left and then clicking find more apps: <br/><br />
<img src="https://i.imgur.com/dCHDxJi.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I searched up the app that I wanted, the app being Splunk Add-on for Microsoft Windows. This app will allow me to use many useful fields when I create alerts later on: <br/><br />
<img src="https://i.imgur.com/5QC48ou.png" alt="Homelab Steps">
<br />
<br />
<br />
It then prompted me to login before installing. Once I did so I let it install and then closed out the prompt: <br/><br />
<img src="https://i.imgur.com/FTIgc9S.png" alt="Homelab Steps">
<br />
<br />
<br />
I then clicked settings, in the top right, and selected indexes: <br/><br />
<img src="https://i.imgur.com/LuOkxoy.png" alt="Homelab Steps">
<br />
<br />
<br />
Then I clicked new index and named it dooley-ad. This index will allow me to identify any event that comes from the Client and DC VMs: <br/><br />
<img src="https://i.imgur.com/yybvY6a.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I clicked settings and selected forwarding and receiving: <br/><br />
<img src="https://i.imgur.com/LtH4oqT.png" alt="Homelab Steps">
<br />
<br />
<br />
From here, I clicked configure receiving. Setting up recieving will allow Splunk Enterprise to recieve the information from the othe VMs: <br/><br />
<img src="https://i.imgur.com/Y5b3JXv.png" alt="Homelab Steps">
<br />
<br />
<br />
Then, I clicked new recieving port: <br/><br />
<img src="https://i.imgur.com/zlIBwF6.png" alt="Homelab Steps">
<br />
<br />
<br />
Finally, I typed in port 9997 as the receiving port and clicked save: <br/><br />
<img src="https://i.imgur.com/EhALGSB.png" alt="Homelab Steps">
<br />
<br />
<br />
The next step was to install the Splunk Universal Forwarder on the Client VM. I went back to Splunk's website, clicked trials & downloads, then selected the universal forwarder: <br/><br />
<img src="https://i.imgur.com/1LQY8W6.png" alt="Homelab Steps">
<br />
<br />
<br />
Then I grabbed the download command for the Windows Server 2022 version: <br/><br />
<img src="https://i.imgur.com/FTlszwm.png" alt="Homelab Steps">
<br />
<br />
<br />
After switching over to the Client VM, I was able to paste the command in the command line. Once it installed, I ran the exe: <br/><br />
<img src="https://i.imgur.com/2swd35N.png" alt="Homelab Steps">
<br />
<br />
<br />
First, I select an on-premises Splunk Enterprise instance and click next: <br/><br />
<img src="https://i.imgur.com/v2mjwTa.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I set up a username and password for the administrator account: <br/><br />
<img src="https://i.imgur.com/9Hcn5r3.png" alt="Homelab Steps">
<br />
<br />
<br />
I continued to click next until it asked me to set up the recieving IP. For this I just entered the Private IP of the Splunk VM on port 9997: <br/><br />
<img src="https://i.imgur.com/eb1Rjek.png" alt="Homelab Steps">
<br />
<br />
<br />
Once that was done I clicked install: <br/><br />
<img src="https://i.imgur.com/YOxpSGG.png" alt="Homelab Steps">
<br />
<br />
<br />
It then prompted me for the Domain Controller Credentials: <br/><br />
<img src="https://i.imgur.com/bME8J5x.png" alt="Homelab Steps">
<br />
<br />
<br />
After all of that, the installation was finished and I went ahead and clicked the finish button: <br/><br />
<img src="https://i.imgur.com/ckKinJA.png" alt="Homelab Steps">
<br />
<br />
<br />
Next I had to configure a few things. I started by entering the C:/Program Files/SplunkUniversalForwarder/etc/system/default folder and copied the inputs.conf file: <br/><br />
<img src="https://i.imgur.com/Xh0wl1W.png" alt="Homelab Steps">
<br />
<br />
<br />
Then, I pasted the inputs.conf file into the C:/Program Files/SplunkUniversalForwarder/etc/system/local folder: <br/><br />
<img src="https://i.imgur.com/D8bnGps.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I needed to edit the inputs.conf file that I had just created. I did so by running notepad as administrator: <br/><br />
<img src="https://i.imgur.com/crvWcMz.png" alt="Homelab Steps">
<br />
<br />
<br />
At the bottom of the file, I added these three lines. Essentially, these lines tell the forwarder to collect windows security events and file them under the dooley-ad index: <br/><br />
<img src="https://i.imgur.com/V2iN5QJ.png" alt="Homelab Steps">
<br />
<br />
<br />
After this, I opened up Services as the administrator: <br/><br />
<img src="https://i.imgur.com/yifqLSf.png" alt="Homelab Steps">
<br />
<br />
<br />
Within the Services application, I double-clicked the SplunkForwarder, clicked the log on tab, and selected the local system option: <br/><br />
<img src="https://i.imgur.com/TPj2OOE.png" alt="Homelab Steps">
<br />
<br />
<br />
After making that change, I right-clicked the SplunkForwarder and restarted it. I then repeated the entire process of installing and configuring the Splunk Universal Forwarder on the Domain Controller VM: <br/><br />
<img src="https://i.imgur.com/oAveVXQ.png" alt="Homelab Steps">
<br />
<br />
<br />
Switching back to the Splunk Enterprise page, I clicked apps in the top left, and then selected search & reporting: <br/><br />
<img src="https://i.imgur.com/rpLM8QK.png" alt="Homelab Steps">
<br />
<br />
<br />
By searching for index="dooley-ad" in the search bar, I was able to confirm that logs were being forwarded correctly: <br/><br />
<img src="https://i.imgur.com/BYKxAyV.png" alt="Homelab Steps">
<br />
<br />
<br />
In addition, by clicking the ComputerName field in the left column, I could confirm that logs were coming from both VMs: <br/><br />
<img src="https://i.imgur.com/VNq1gRl.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I wanted to craft an alert. Essentially, this alert looks within the dooley-ad index for event 4624 (Successful Logon). It then filters out all logon types except 7 and 10. Type 7 is when a user has unlocked the workstation and Type 10 is when the logon was from Remote Desktop Protocol. The alert then filters out any events that have unspecified address or internal IPs. Finally, the alert summarizes the filtered data by grouping and counting events using the specified fields: <br/><br />
<img src="https://i.imgur.com/qKrv9Kt.png" alt="Homelab Steps">
<br />
<br />
<br />
Once I had made the alert, I clicked save as alert in the right corner: <br/><br />
<img src="https://i.imgur.com/LyT49eh.png" alt="Homelab Steps">
<br />
<br />
<br />
I went ahead and named the alert something that would be obvious. Then, I configured the alert to run every minute and had it get added to triggered alerts every time it was detected. Adding it to the triggered alerts was used later in the project to configure Shuffle: <br/><br />
<img src="https://i.imgur.com/JW7GSOq.png" alt="Homelab Steps">
<br />
<br />
<br />
This was all for setting up Splunk. The project documentation is continued in the Wazuh Steps: <br/><br />
https://github.com/rdooley2/Homelab/blob/main/Wazuh.md
