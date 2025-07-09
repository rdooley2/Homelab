<p align="center">
The next part of the project was to set up Wazuh. First, I used SSH to get into the Wazuh-Suricata VM and then I ran apt update and apt upgrade: <br/><br />
<img src="https://i.imgur.com/p6N5JsP.png" alt="Homelab Steps">
<br />
<br />
<br />
After the updates installed, I rebooted the select services: <br/><br />
<img src="https://i.imgur.com/UzGmLls.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I installed Wazuh using this command. The command comes straight from Wazuh's quickstart guide online. Once it installed, it gave me the URL and port extension to access the Wazuh web interface: <br/><br />
<img src="https://i.imgur.com/uw5HAY8.png" alt="Homelab Steps">
<br />
<br />
<br />
I hadn't accounted for using port 443 in my research before starting the project, so I had to go back to the firewall settings on the Vultr website and add that specific port: <br/><br />
<img src="https://i.imgur.com/L227nOF.png" alt="Homelab Steps">
<br />
<br />
<br />
Back inside the SSH instance, I ran the following commands to allow the ports I had specified before: <br/><br />
<img src="https://i.imgur.com/ZJwShL4.png" alt="Homelab Steps">
<br />
<br />
<br />
Once that was done, I downloaded the Wazuh Agent installer for Windows from this page: <br/><br />
<img src="https://i.imgur.com/ZjqTB1n.png" alt="Homelab Steps">
<br />
<br />
<br />
After it downloaded, I navigated to the Client Vm where I wanted to install the agent: <br/><br />
<img src="https://i.imgur.com/83GVdpg.png" alt="Homelab Steps">
<br />
<br />
<br />
After pasting over the installer I was able to start the installation of the agent: <br/><br />
<img src="https://i.imgur.com/cak7iGE.png" alt="Homelab Steps">
<br />
<br />
<br />
When the installer finished, I clicked this button to open the configuration screen: <br/><br />
<img src="https://i.imgur.com/ba2aeYi.png" alt="Homelab Steps">
<br />
<br />
<br />
On this screen, I specified the Private IP of the Wazuh-Suricata VM so that the agent knows where to communicate: <br/><br />
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
Clicking the Client brought me to this page were I could view all sorts of information that the agent had gathered: <br/><br />
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
Going back to the SSH instance of the Wazuh-Suricata VM, I needed to set up the Splunk Forwarder. I ran this wget command to grab the .deb file, then I depackaged it, and finally I accepted the license. I will mention there was another step that I took after these three commands that I forgot to screenshot. In this step, I ran the command "/opt/splunkforwarder/bin/splunk add forward-server 10.1.96.5:9997" in order to tell the forwarder where the information should be sent: <br/><br />
<img src="https://i.imgur.com/idnXD8L.png" alt="Homelab Steps">
<br />
<br />
<br />
Next I created the inputs.conf in the /opt/splunkforwarder/etc/system/local/ folder, similar to when I set up the forwarder on the other two VMs, and added the following lines. Essentially it tells Splunk to watch and forward all data from the file at that file path since that is where all detected Wazuh events are kept. It then tells Splunk to categorize those events under the correct index, host, and sourcetype : <br/><br />
<img src="https://i.imgur.com/lrX8Gsm.png" alt="Homelab Steps">
<img src="https://i.imgur.com/SYrC3Dq.png" alt="Homelab Steps">
<br />
<br />
<br />
After saving the file changes, I restarted the forwarder so that the changes could apply: <br/><br />
<img src="https://i.imgur.com/qPAiUbM.png" alt="Homelab Steps">
<br />
<br />
<br />
Switching back to Splunk Enterprise, I confirmed that everything was set up right by searching up the wazuh-alerts index: <br/><br />
<img src="https://i.imgur.com/WVohj3O.png" alt="Homelab Steps">
<br />
<br />
<br />
Next I wanted to set up Wazuh File Integrity Monitoring. To do so, I navigated to the ossec.conf file within C:/Program Files (x86)/ossec-agent/ folder and edited it with notepad. I added a directories line and had it monitor the Downloads folder for all users (hence the wildcard symbol *). I also set the recursion level to one. The recursion_level attribute controls how many levels deep Wazuh will monitor subdirectories, in this case only one folder deep. I also set the frequency to 60 (every minute) for testing purposes: <br/><br />
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
Back in Splunk Enterprise, I created an query for all events from Wazuh that contained the word files and had the words added or deleted. This is because all changes are categorized as added or deleted in the event log. If I did not specify "added" or "deleted", the query would pull other detected events from the FIM. The query is designed to pull the event time, full log (log description), and the agent the event is from: <br/><br />
<img src="https://i.imgur.com/P3cKIQa.png" alt="Homelab Steps">
<br />
<br />
<br />
Finally, I saved the alert as Dooley-File-Change-FIM. I used the same settings that I did in the previous alert, with the expection of the severity being lower: <br/><br />
<img src="https://i.imgur.com/LScyTY0.png" alt="Homelab Steps">
<br />
<br />
<br />
This was all for setting up Wazuh. The project documentation is continued in the Suricata Steps: <br/><br />
https://github.com/rdooley2/Homelab/blob/main/Suricata.md
