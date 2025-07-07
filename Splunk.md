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
I had to go in and set each port that I wanted to use as allowed for the firewall: <br/><br />
<img src="https://i.imgur.com/b0rJqGZ.png" alt="Homelab Steps">
<br />
<br />
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
Next, I searched up the app that I wanted, the app being Splunk Add-on for Microsoft Windows: <br/><br />
<img src="https://i.imgur.com/5QC48ou.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/4ceDYop.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/S4hKmP8.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/EDXLTYa.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/SD1476V.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/bPrHGer.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/LSdETxb.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/houlKrL.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/79Tp6EA.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/2XGXlwa.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/6SMAaPR.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/UMlGjQh.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/12roRk0.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/j78hGpY.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/otaqz2V.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/tg9VHyK.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/NJOHlBw.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/hRvk2wm.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/fZo4PDH.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/m0Arr3G.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/MlWo0I5.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/OoDfgoh.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/4BdPB2n.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/3QRzMIV.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/mavBBnu.png" alt="Homelab Steps">
<br />
<br />
<br />
: <br/><br />
<img src="https://i.imgur.com/4P9S9Cr.png" alt="Homelab Steps">
<br />
<br />
<br />
This was all for setting up Splunk. The project documentation is continued in the Wazuh Steps: <br/><br />
https://github.com/rdooley2/Homelab/blob/main/Wazuh.md
