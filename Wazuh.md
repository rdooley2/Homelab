<h2>Project Index</h2>

- <b>[Virtual Machine Steps](https://github.com/rdooley2/Homelab/blob/main/README.md)</b>
- <b>[Active Directory Steps](https://github.com/rdooley2/Homelab/blob/main/ActiveDirectory.md)</b>
- <b>[Network Drive Steps](https://github.com/rdooley2/Homelab/blob/main/NetworkDrive.md)</b>
- <b>[Splunk Steps](https://github.com/rdooley2/Homelab/blob/main/Splunk.md)</b>
- <b>[Wazuh Steps](https://github.com/rdooley2/Homelab/blob/main/Wazuh.md)</b>
- <b>[Suricata Steps](https://github.com/rdooley2/Homelab/blob/main/Suricata.md)</b>
- <b>[Shuffle & Slack Steps](https://github.com/rdooley2/Homelab/blob/main/Shuffle&Slack.md)</b>
- <b>[Dashboard Steps](https://github.com/rdooley2/Homelab/blob/main/Dashboard.md)</b><br>

<h2>Wazuh Steps</h2>
<p align="center">
The next part of the project was to set up Wazuh. First, I used SSH to get into the Wazuh VM, and then I ran these commands to update packages. After the updates were installed, I rebooted the affected services: <br/><br />
<pre>
apt update && apt upgrade
</pre>
<br />
<p align="center">
Next, I had to go in and set each port that I wanted to use, as allowed by the firewall. These are the commands that I ran: <br/><br />
<pre>
ufw allow 22
ufw allow 443
ufw allow 1514
ufw allow 1515
ufw allow 9997
</pre>
<p align="center">
<br />
Next, I installed Wazuh using this command. The command comes straight from Wazuh's quickstart guide online. Once it was installed, it gave me the URL and port extension to access the Wazuh web interface: <br/><br />
<pre>
curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh && sudo bash ./wazuh-install.sh -a
</pre>
<br />
<p align="center">
Once that was done, I downloaded the Wazuh Agent installer from Wazuh's website. After it downloaded, I navigated to the Client VM where I wanted to install the agent: <br/><br />
<img src="https://i.imgur.com/83GVdpg.png" alt="Homelab Steps">
<br />
<br />
<br />
After pasting over the installer, I was able to start the installation of the agent: <br/><br />
<img src="https://i.imgur.com/cak7iGE.png" alt="Homelab Steps">
<br />
<br />
<br />
When the installer finished, I clicked this button to open the configuration screen: <br/><br />
<img src="https://i.imgur.com/ba2aeYi.png" alt="Homelab Steps">
<br />
<br />
<br />
On this screen, I specified the Private IP of the Wazuh VM so that the agent knows where to communicate: <br/><br />
<img src="https://i.imgur.com/60qqnWk.png" alt="Homelab Steps">
<br />
<br />
<br />
After saving the manager IP, I clicked manage and started the agent: <br/><br />
<img src="https://i.imgur.com/xVbgdlC.png" alt="Homelab Steps">
<br />
<br />
<br />
In the Client VM, I went to Microsoft Edge and typed in the URL from before to access the Wazuh dashboard: <br/><br />
<img src="https://i.imgur.com/yplxyHr.png" alt="Homelab Steps">
<br />
<br />
<br />
Upon successful logon, I clicked the active button at the top left of the page: <br/><br />
<img src="https://i.imgur.com/SsjMqYo.png" alt="Homelab Steps">
<br />
<br />
<br />
I was brought to a page that showed all active agents. I was able to confirm that the agent and Wazuh had linked up correctly. Then, I clicked the Client to see more information: <br/><br />
<img src="https://i.imgur.com/32SMC1U.png" alt="Homelab Steps">
<br />
<br />
<br />
Clicking the Client brought me to this page where I could view all sorts of information that the agent had gathered: <br/><br />
<img src="https://i.imgur.com/LwepKoX.png" alt="Homelab Steps">
<br />
<br />
<br />
With Wazuh installed and monitoring the Client VM, I navigated to Splunk Enterprise to start the process of connecting it with Wazuh. First, I clicked settings in the top right and selected indexes: <br/><br />
<img src="https://i.imgur.com/pTZiQ2T.png" alt="Homelab Steps">
<br />
<br />
<br />
This time I named the index wazuh-alerts so that I could differentiate between wazuh-alerts logs and dooley-ad logs: <br/><br />
<img src="https://i.imgur.com/WtFD3K5.png" alt="Homelab Steps">
<br />
<br />
<br />
Going back to the SSH instance of the Wazuh VM, I needed to set up the Splunk Forwarder. I ran these commands to do so: <br/><br />
<pre>
wget -O splunkforwarder.deb "https://download.splunk.com/products/universalforwarder/releases/9.1.1/linux/splunkforwarder-9.1.1-64e843ea36b1-linux-2.6-amd64.deb"       #Download the Forwarder
dpkg -i splunkforwarder.deb                                                                                                                                             #Depackage the Forwarder
/opt/splunkforwarder/bin/splunk start --accept-license                                                                                                                  #Start installation and accept license
</pre>
<p align="center">
<br />
For the next step, I ran this command to tell the forwarder where the information should be sent: <br/><br />
<pre>
/opt/splunkforwarder/bin/splunk add forward-server 10.1.96.5:9997         #Adds the Splunk Enterprise IP into the Splunk Forwarder Configuration
</pre>
<p align="center">
<br /> 
Next, I created the inputs.conf file, similar to when I set the forwarder up on the Client and DC Virtual Machines, and added the following lines. Once the file was added, I restarted the Forwarder: <br/><br />
<pre>
vi /opt/splunkforwarder/etc/system/local/inputs.conf
<br/>
[monitor:///var/ossec/logs/alerts/alerts.json]         #Tells forwarder to send all data from this path to the Splunk Virtual Machine
disabled = 0                    
host = wazuh-server                                    #Set the event host equal to wazuh-server
index = wazuh-alerts                                   #Set the index equal to wazuh-alerts
sourcetype = wazuh-alerts                              #Set the sourcetype equal to wazuh-alerts
<br/>
/opt/splunkforwarder/bin/splunk restart                #Restarts the Splunk Forwarder to apply changes
</pre>
<p align="center">
<br />
Switching back to Splunk Enterprise, I confirmed that everything was set up right by searching the wazuh-alerts index: <br/><br />
<img src="https://i.imgur.com/WVohj3O.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I wanted to set up Wazuh File Integrity Monitoring. To do so, I navigated to the ossec.conf file within C:/Program Files (x86)/ossec-agent/ folder and edited it with Notepad. I added a directory line and had it monitor the Downloads folder for all users (hence the wildcard symbol *). I also set the recursion level to one. The recursion_level attribute controls how many levels deep Wazuh will monitor subdirectories; in this case, only one folder deep. I also set the frequency to 60 (every minute) for testing purposes: <br/><br />
<img src="https://i.imgur.com/psy5ofG.png" alt="Homelab Steps">
<br />
<br />
<br />
Finally, I navigated to the Services application and restarted Wazuh to apply the changes to the ossec.conf file: <br/><br />
<img src="https://i.imgur.com/ohexJPs.png" alt="Homelab Steps">
<br />
<br />
<br />
To test the FIM, I created two files in the download folder: <br/><br />
<img src="https://i.imgur.com/4UpbfLl.png" alt="Homelab Steps">
<br />
<br />
<br />
I was able to confirm that those changes were caught and displayed in the Wazuh dashboard: <br/><br />
<img src="https://i.imgur.com/Y9Ofhyp.png" alt="Homelab Steps">
<br />
<br />
<br />
Back in Splunk Enterprise, I created a query for all events from Wazuh that contained the word files and had the words added or deleted. This is because all file-specific changes are categorized as added or deleted by the FIM. If I did not specify "added" or "deleted", the query would pull other detected events from the FIM. This query is designed to pull the event time, full log (log description), and the agent the event is from: <br/><br />
<img src="https://i.imgur.com/P3cKIQa.png" alt="Homelab Steps">
<br />
<br />
<br />
Finally, I saved the alert as Dooley-File-Change-FIM. I used the same settings that I did in the previous alert, except for the severity being lower: <br/><br />
<img src="https://i.imgur.com/LScyTY0.png" alt="Homelab Steps">
<br />
<br />
<br />
This was all for setting up Wazuh. The project documentation is continued in the Suricata Steps: <br/><br />
https://github.com/rdooley2/Homelab/blob/main/Suricata.md
