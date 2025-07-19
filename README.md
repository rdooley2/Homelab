<h1>Homelab</h1>


<h2>Overview</h2>
I used Vultr to create my own Homelab and simulated attacks to test the software. This project aimed to develop my skills in the following areas and software:
<br><br>
<ul>
 <li>Active Directory - Windows Server</li>
 <li>Setting up Network Drives - Windows Server/Active Directory</li>
 <li>Firewall Setup/Configuration - Vultr</li>
 <li>Network-Based Intrusion Detection System (NIDS)/Network-Based Intrusion Prevention System (NIPS) - Suricata</li>
 <li>Extended Detection and Response (XDR) - Wazuh</li>
 <li>Vulnerability Detection - Wazuh</li>
 <li>File Integrity Monitoring (FIM) - Wazuh</li>
 <li>Security Information and Event Management (SIEM) - Splunk</li>
 <li>Security Orchestration, Automation, and Response (SOAR) - Slack/Shuffle</li>
</ul>

<h2>Summary</h2>
In this project, I used Vultr to create multiple Virtual Machines and set up a homelab. Each VM was configured with different software, allowing for various features and protections. First, I set up the Domain Controller and Client VMs with multiple users, groups, and organizational units. Second, I set up Network Drives for each user and group for file sharing. Third, I set up Splunk to work on the third VM and installed Splunk Forwarders on the DC, Client, and Wazuh VMs to forward Security Events/Alerts. Fourth, I installed Wazuh on the fourth VM and installed the Wazuh agent on the Client and Suricata VMs to ingest logs that would eventually be sent to Splunk. Fifth, I set up Suricata on the fifth VM and configured it to send logs to the Wazuh VM. Finally, I set up alert automation with Shuffle so that when alerts were triggered in Splunk, a text would be sent in Slack to simulate a work environment.


<h2>Languages and Utilities Used</h2>

- <b>Active Directory</b>
- <b>Splunk</b>
- <b>Wazuh</b>
- <b>Suricata</b>

<h2>Environments Used </h2>

- <b>[Active Directory Steps](https://github.com/rdooley2/Homelab/blob/main/ActiveDirectory.md)</b>
- <b>[Network Drive Steps](https://github.com/rdooley2/Homelab/blob/main/NetworkDrive.md)</b>
- <b>[Splunk Steps](https://github.com/rdooley2/Homelab/blob/main/Splunk.md)</b>
- <b>[Wazuh Steps](https://github.com/rdooley2/Homelab/blob/main/Wazuh.md)</b>
- <b>[Suricata Steps](https://github.com/rdooley2/Homelab/blob/main/Suricata.md)</b>
- <b>[Shuffle & Slack Steps](https://github.com/rdooley2/Homelab/blob/main/Shuffle&Slack.md)</b>
- <b>[Dashboard Steps](https://github.com/rdooley2/Homelab/blob/main/Dashboard.md)</b>

<h2>Project Index</h2>

- <b>Vultr</b>
- <b>Shuffle</b>
- <b>Slack</b>

<h2>Project walk-through:</h2>

<p align="center">
To start the project, I took time to research and design a Network Map. Here is what I came up with: <br/><br />
<img src="https://i.imgur.com/Byrxdd4.png" alt="Homelab Steps">
<br />
<br />
<br />
First, I navigated to Vultr's website and registered for a new account. Once I did so, I was given $300 worth of free credit to use for my project. The first step in the project was to initialize the VMs, so I went to the products tab and clicked deploy server: <br/><br />
<img src="https://i.imgur.com/GqitfmU.png" alt="Homelab Steps">
<br />
<br />
<br />
Here are the specifications for each Virtual Machine:
<ul>
 <li>Domain-Controller / Windows 2022 Standard x64 / 2 CPU Cores / 4 GB Memory / 80 GB Storage / Automatic Backups Disabled</li>
 <li>Client / Windows 2022 Standard x64 / 1 CPU Core / 2 GB Memory / 55 GB Storage / Automatic Backups Disabled</li>
 <li>Splunk / Ubuntu 22.04 LTS x64 / 4 CPU Cores / 8 GB Memory / 160 GB Storage / Automatic Backups Disabled</li>
 <li>Wazuh / Ubuntu 22.04 LTS x64 / 2 CPU Cores / 4 GB Memory / 100 GB Storage / Automatic Backups Disabled</li>
 <li>Suricata / Ubuntu 22.04 LTS x64 / 2 CPU Cores / 4 GB Memory / 100 GB Storage / Automatic Backups Disabled</li>
</ul>
<p align="center">
<br />
Returning to the product page, I was now able to see that all five VMs had initialized. From here, I navigated to the Firewall tab, which was under the Network tab: <br/><br />
<img src="https://i.imgur.com/TTrI7oZ.png" alt="Homelab Steps">
<br />
<br />
<br />
I wanted to create a Firewall Group that I could apply to all five VMs. For testing reasons, I only created one Firewall for the time being. I planned to create multiple specific Firewalls once I started working on Suricata and knew that everything worked. I started by clicking add firewall group: <br/><br />
<img src="https://i.imgur.com/ypHVddz.png" alt="Homelab Steps">
<br />
<br />
<br />
The ports I added are as follows:
<ul>
 <li>Port 22 - Enables SSH into any of the Virtual Machines</li>
 <li>Port 443 - This port needs to be enabled to access the Wazuh Dashboard</li>
 <li>Port 1514 - Wazuh Manager uses this port to receive event data</li>
 <li>Port 1515 - Wazuh uses this port for Agent enrollment</li>
 <li>Port 8000 - Splunk Enterprise uses this port to display its GUI</li>
 <li>Port 9997 - Splunk Universal Forwarder uses this port to send data to Splunk Enterprise</li>
</ul>
<p align="center">
<br />
Looking at the settings for the Domain Controller, under the firewall tab, I was able to apply the new firewall to the VM. I then repeated this process for the other four VMs: <br/><br />
<img src="https://i.imgur.com/VnT4YHp.png" alt="Homelab Steps">
<br />
<br />
<br />
<br />
Next, I set up a VPC Network. This essentially creates a Private IP for the machines to communicate with each other. Similar to before, in the settings, I could click VPC network and then click Enable VPC. Doing so generated a private IP: <br/><br />
<img src="https://i.imgur.com/I1gs3dH.png" alt="Homelab Steps">
<br />
<br />
<br />
I then repeated this for the other four VMs and took note of each Public and Private IP: <br/><br />
<img src="https://i.imgur.com/fYYWQT7.png" alt="Homelab Steps">
<br />
<br />
<br /> 
With those out of the way, I could go into the VMs for the first time to configure them. In this case, I went into the Client: <br/><br />
<img src="https://i.imgur.com/s37qx1g.png" alt="Homelab Steps">
<br />
<br />
<br />
When running the ipconfig command in the command prompt, I found that the Client was not using the Private IP that had just been created: <br/><br />
<img src="https://i.imgur.com/zFDlksc.png" alt="Homelab Steps">
<br />
<br />
<br />
To fix this, I went into the Network Status in the settings. From there, I clicked change adapter options, right clicked Ethernet instance 0.2, clicked properties, double clicked on IPv4, and manually had it use the private IP I had previously created: <br/><br />
<img src="https://i.imgur.com/3W5G5ma.png" alt="Homelab Steps">
<br />
<br />
<br />
Rerunning the ipconfig command showed the changes had been applied correctly. I did this same process on the Domain Controller VM as it had the same problem: <br/><br />
<img src="https://i.imgur.com/2i9eu0n.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I used SSH to get into the Splunk VM from my command prompt. Here I pinged the DC, Client, Wazuh, and Suricata VMs to ensure they could communicate with each other: <br/><br />
<img src="https://i.imgur.com/IC6b7ns.png" alt="Homelab Steps">
<br />
<br />
<br />
This was all for initializing the VMs. The project documentation is continued in the Active Directory Steps: <br/><br />
https://github.com/rdooley2/Homelab/blob/main/ActiveDirectory.md

