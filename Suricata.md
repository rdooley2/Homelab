<h2>Project Index</h2>

- <b>[Virtual Machine Steps](https://github.com/rdooley2/Homelab/blob/main/README.md)</b>
- <b>[Active Directory Steps](https://github.com/rdooley2/Homelab/blob/main/ActiveDirectory.md)</b>
- <b>[Network Drive Steps](https://github.com/rdooley2/Homelab/blob/main/NetworkDrive.md)</b>
- <b>[Splunk Steps](https://github.com/rdooley2/Homelab/blob/main/Splunk.md)</b>
- <b>[Wazuh Steps](https://github.com/rdooley2/Homelab/blob/main/Wazuh.md)</b>
- <b>[Suricata Steps](https://github.com/rdooley2/Homelab/blob/main/Suricata.md)</b>
- <b>[Shuffle & Slack Steps](https://github.com/rdooley2/Homelab/blob/main/Shuffle&Slack.md)</b>
- <b>[Dashboard Steps](https://github.com/rdooley2/Homelab/blob/main/Dashboard.md)</b><br>

<h2>Suricata Steps</h2>
<p align="center">
The next part of the project was to set up Suricata. In this part of the project, I focused on securing the network. First, I went ahead and made specific firewalls for each Virtual Machine (except the Suricata VM) since I knew everything worked so far. Here are those firewalls:<br/><br />
<img src="https://i.imgur.com/z3Ks5Ah.png" alt="Homelab Steps">
<img src="https://i.imgur.com/YmPdp8l.png" alt="Homelab Steps">
<img src="https://i.imgur.com/mPqyeo7.png" alt="Homelab Steps">
<img src="https://i.imgur.com/QJFnLZy.png" alt="Homelab Steps">
<br />
<br />
<br />
Additionally, I had to update the ufw rules in the Splunk and Wazuh Virtual Machines. Here are the commands I ran to do so:
<pre>
#Splunk VM 
<br/>
ufw reset                            #Resets ufw back to default
<br /> 
ufw default deny incoming            #Set default policy to deny all inbound traffic
ufw default allow outgoing           #Set default policy to deny all outbound traffic
<br /> 
ufw allow from My_IP to any port 22 proto tcp               #Allow SSH from my IP to Splunk 
ufw allow from 10.1.96.0/20 to any port 1514 proto tcp      #Allows Wazuh Dashboard to communicate with Wazuh Agents
ufw allow from My_IP to any port 8000 proto tcp             #Allows my IP address to access the Splunk Enterprise website on this port
ufw allow from 10.1.96.0/20 to any port 9997 proto tcp      #Allows Splunk Forwarder to send data to Splunk Enterprise
<br /> 
ufw enable    #Enables ufw
<br /> 
#Wazuh VM
<br /> 
ufw reset                            #Resets ufw back to default
<br />
ufw default deny incoming            #Set default policy to deny all inbound traffic
ufw default allow outgoing           #Set default policy to deny all outbound traffic
<br />
ufw allow from My_IP to any port 22 proto tcp             #Allow SSH from my IP to Splunk
ufw allow from 10.1.96.0/20 to any port 443 proto tcp     #Allows HTTP from any of the Private IPs to the Wazuh Dashboard 
ufw allow from 10.1.96.0/20 to any port 1514 proto tcp    #Allows Wazuh Dashboard to communicate with Wazuh Agents
ufw allow from 10.1.96.0/20 to any port 9997 proto tcp    #Allows Splunk Forwarder to send data to Splunk Enterprise
<br />
ufw enable                                                #Enables ufw
</pre>
<p align="center">
I also wanted to disable the public IPs for the Virtual Machines that didn't need them (Wazuh, DC, and Client). Since they only need to communicate with each other, I went ahead and started that process. While I was in the Wazuh VM, I went ahead and ran this command to disable the Public Network Interface:
<pre>
ip link set enp1s0 down      #Disables enp1s0 (Public IP)
</pre>
<br />
<p align="center">
Switching over to the Client, I went to network status and changed the default gateway for the private ethernet instance equal to the private IP of the Suricata VM: <br/><br />
<img src="https://i.imgur.com/FaKBJoV.png" alt="Homelab Steps">
<br />
<br />
<br />
Additionally, I disabled IPv4 completely within the settings of the public Ethernet instance. This means the only inbound traffic the Client receives is in response to a previous outbound request. Additionally, it forces all outbound traffic to run through the Suricata VM before reaching its destination. This way, all traffic can be monitored by Suricata. I repeated this process for the DC VM: <br/><br />
<img src="https://i.imgur.com/CN5knZr.png" alt="Homelab Steps">
<br />
<br />
<br />
<p align="center">
Next, in the Suricata VM, I needed to enable IP Forwarding. First, I ran these commands:
<pre>
vi /etc/sysctl.conf                                     #Edit configuration file
Uncomment "net.ipv4.ip_forward = 1" line                #This variable allows IP Forwarding 
sysctl -p                                               #Restart to apply changes           
</pre>
<br />
Next, I needed to know the network interfaces to correctly set up the IP forwarding. I ran this command to find which interface corresponded to each IP: <br/><br />
<pre>
ip a
</pre>
<br />
<p align="center">
Using the interfaces I found, I ran these commands to correctly configure the IP tables. These tables are responsible for controlling network traffic on the VM:
<pre>
iptables -t nat -A POSTROUTING -o enp1s0 -j MASQUERADE                                              #Hide Private IP (enp8s0) requests behind the Public IP counterpart (enp1s0)
iptables -A FORWARD -i enp8s0 -o enp1s0 -j ACCEPT                                                   #Allow outbound traffic
iptables -A FORWARD -i enp1s0 -o enp8s0 -m state --state RELATED,ESTABLISHED -j ACCEPT              #Allow inbound traffic
</pre>
<p align="center">
For these changes to apply after a restart, I ran these commands to download a module to save the changes:
<pre>
sudo apt install iptables-persistent 
sudo netfilter-persistent save 
</pre>
<br />
<p align="center">

To test these changes, I ran the trace route command in the Client CLI. The results showed the traffic going through the Suricata VM (10.1.96.7) first before reaching the destination: <br/><br />
<img src="https://i.imgur.com/y4PVUhc.png" alt="Homelab Steps">
<br />
<br />
<br />
<p align="center">
Going back to the Suricata VM, the next step was to download the Wazuh agent and configure it. These commands come straight from their Linux agent installation guide:
<pre>
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | gpg --no-default-keyring --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import && chmod 644 /usr/share/keyrings/wazuh.gpg       #Installs the GPG key
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" | tee -a /etc/apt/sources.list.d/wazuh.list                                             #Adds the Wazuh Repository
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
  
default-rule-path: /etc/suricata/rules          #Specify new rule path I created in the previous step
rule-files:
  "*.rules"                                     #Load all rules within the specified path

af-packet:
  interface: enp1s0                             #Set interface to one that correlates with public IP

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
To test that everything was working, I installed Nmap for Windows on my personal computer and ran this port scan to trigger an alert: <br/><br />
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
This was all for setting up Suricata. The project documentation is continued in the Shuffle & Slack Steps: <br/><br />
https://github.com/rdooley2/Homelab/blob/main/Shuffle%26Slack.md
