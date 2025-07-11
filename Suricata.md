<p align="center">
The first thing that I did was create a new VM for Suricata to run on. I realized that Suricata only integrates with Wazuh through the Wazuh agent. Essentially, I could install an agent on the new VM and have Suricata send the agent its logs. While I didn't initlally didn't plan for this, it makes more sense to do it this way. Here is the updated network diagram:<br/><br />
<img src="" alt="Homelab Steps">
<br />
<br />
<br />  
These are the specifications that I used for the Suricata VM, It is the exact same as the Wazuh-Suricata VM (which I renamed to Wazuh VM): <br/><br />
<img src="https://i.imgur.com/02bl9m7.png" alt="Homelab Steps">
<br />
<br />
<br />
After the new VM initialized I enabled the firewall and VPC network for it. Once that was done, I recorded the new public and private IP: <br/><br />
<img src="https://i.imgur.com/7U3siyU.png" alt="Homelab Steps">
<br />
<br />
<br />
After using SSH to get into the machine, I ran these commands to allow each port in the firewall:
<pre>
ufw allow 22
ufw allow 389
ufw allow 1514
ufw allow 1515
ufw allow 3389
ufw allow 8000
ufw allow 9997
</pre>
</pre>
<p align="center">
Next I wanted to enable IP Forwarding. This makes sure all inbound and outbound traffic run through the Suricata VM before reaching its destination. First, I ran these commands:
<pre>
vi /etc/sysctl.conf                                     #Edit configuration file
Uncomment "net.ipv4.ip_forward = 1" line                #This variable allows IP Forwarding 
sysctl -p                                               #Restart to apply changes           
</pre>
<br />
<p align="center">
In order to correctly set up the forwarding, I needed to know the network interfaces. I ran the "ip a" command to find which interface cooresponded to each IP: <br/><br />
<img src="https://i.imgur.com/weVe81p.png" alt="Homelab Steps">
<br />
<br />
<br />
<p align="center">
Using the interfaces I just found, I ran these commands to correctly configure the IP tables. These tables are responsible for controlling network traffic on the VM:
<pre>
iptables -t nat -A POSTROUTING -o enp1s0 -j MASQUERADE        #Hide Private IP requests behind their Public IP counterparts
iptables -A FORWARD -i enp8s0 -o enp1s0 -j ACCEPT                                                   #Allow outbound traffic
iptables -A FORWARD -i enp1s0 -o enp8s0 -m state --state RELATED,ESTABLISHED -j ACCEPT               #Allow inbound traffic
</pre>
<p align="center">
For these changes to apply after restart, I ran these commands to download a module to save the changes:
<pre>
sudo apt install iptables-persistent 
sudo netfilter-persistent save 
</pre>
<br />
<p align="center">
Switching over to the Client, I went to network status and changed the default gateway for the private ethernet instance equal to the private IP of the Suricata VM: <br/><br />
<img src="https://i.imgur.com/ZSWGaD1.png" alt="Homelab Steps">
<br />
<br />
<br />
Additionally, I disabled IPv4 completely within the settings of the public ethernet instance. This will ensure that traffic has no other choice but to run through the Suricata VM: <br/><br />
<img src="https://i.imgur.com/CN5knZr.png" alt="Homelab Steps">
<br />
<br />
<br />
To test these changes, I ran the trace route command in the Client CLI. The results showed the traffic going through the Suricata VM (10.1.96.7) first before reaching the destination. I repeated these same steps for the DC VM: <br/><br />
<img src="https://i.imgur.com/CN5knZr.png" alt="Homelab Steps">
<br />
<br />
<br />
<p align="center">
The next step was to download the Wazuh agent and configure it. These commands come straight from their Linux agent installation guide:
<pre>
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && chmod 644 /usr/share/keyrings/wazuh.gpg         #Installs the GPG key
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | tee -a /etc/apt/sources.list.d/wazuh.list                                          #Adds the Wazuh Repository
apt-get update
WAZUH_MANAGER="10.1.96.6" apt-get install wazuh-agent               #Installs the agent
systemctl daemon-reload                                             #Reloads daemon to apply changes 
systemctl enable wazuh-agent                                        #Enables the agent
systemctl start wazuh-agent                                         #Starts the agent
</pre>
<br />
<p align="center">
Navigating over to the Wazuh dashboard, I could confirm that the agent was working: <br/><br />
<img src="https://i.imgur.com/q1jPWUy.png" alt="Homelab Steps">
<br />
<br />
<br />
<p align="center">
The final step was to install Suricata. First, I ran these commands:
<pre>
add-apt-repository ppa:oisf/suricata-stable        #Adds the Suricata Repository
apt-get update 
apt-get install suricata -y                        #Installs Suricata
</pre>
<p align="center">
Then I ran these commands to download the Emerging Threats Ruleset. This is the particular ruleset that I chose to use for this part of the project:
<pre>
cd /tmp/ && curl -LO https://rules.emergingthreats.net/open/suricata-6.0.8/emerging.rules.tar.gz                           #Installs the Emerging Threats Ruleset
tar -xvzf emerging.rules.tar.gz && mkdir /etc/suricata/rules && mv rules/*.rules /etc/suricata/rules/                      #Makes a new directory and stores the ruleset there
chmod 640 /etc/suricata/rules/*.rules                                                                                      #Assigns correct permissions for the rules
chmod -R 640 /etc/suricata/rules/*
chmod 750 /etc/suricata/rules
</pre>
<p align="center">
Next, I ran this command to edit the suricata.yaml file. I changed the values to look like the ones below and restarted Suricata to apply the changes:
<pre>
vi /etc/suricata/suricata.yaml
<br/>  
HOME_NET: "[207.148.16.92/23]"                  #Set HOME_NET equal to the Suricata public IP
  
EXTERNAL_NET: "any"
  
default-rule-path: /etc/suricata/rules          #Specify new rule path I created in previous step
rule-files:
  "*.rules"                                     #Load all rules within the specified path

af-packet:
  interface: enp1s0                             #Set interface to one that correalates with public IP

systemctl restart suricata                      #Restart Suricata
</pre>
<p align="center">
Finally, I navigated to the Wazuh Agent's ossec.conf file and added the following code. Then I restarted the agent to apply the changes. This code is crucial because it tells the agent where to look for the Suricata logs:
<pre>
vi /var/ossec/etc/ossec.conf      
<br/>
&lt;localfile&gt;
  &lt;log_format&gt;json&lt;/log_format&gt;
  &lt;location&gt;/var/log/suricata/eve.json&lt;/location&gt;
&lt;/localfile&gt;
<br/>
systemctl restart wazuh-agent
</pre>
<br />
<p align="center">
To test that everything was working, I turned off the firewall for the Suricata VM. I also installed Nmap for Windows on my personal computer and ran this port scan to trigger an alert: <br/><br />
<img src="https://i.imgur.com/Bp6Wp0P.png" alt="Homelab Steps">
<br />
<br />
<br />
Navigating to my Wazuh dashboard, I could see that the alerts were showing up. This meant everything was working as planned: <br/><br />
<img src="https://i.imgur.com/ctgWYRw.png" alt="Homelab Steps">
<br />
<br />
<br />
Navigating to my Splunk Enterprise dashboard, I went ahead and made a query that would detect those Nmap scans: <br/><br />
<img src="https://i.imgur.com/HjMQ4Kf.png" alt="Homelab Steps">
<br />
<br />
<br />
Finally, I saved this query as an alert with the same settings as the other two alerts: <br/><br />
<img src="https://i.imgur.com/j5a2Sl5.png" alt="Homelab Steps">
<br />
<br />
<br />
This was all for setting up Suricata. The project documentation is continued in the Shuffle/Slack Steps: <br/><br />
