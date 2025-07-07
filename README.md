<h1>Homelab</h1>


<h2>Overview</h2>
I used Vultr to create my own Homelab and simulate attacks. This project aimed to achieve the following goals:
<br><br>
<ul>
 <li></li>
 <li></li>
 <li></li>
 <li></li>
</ul>

<h2>Summary</h2>
In this project, 

<h2>Languages and Utilities Used</h2>

- <b></b>

<h2>Environments Used </h2>

- <b>Vultr</b>

<h2>Project walk-through:</h2>

<p align="center">
: <br/><br />
<img src="https://i.imgur.com/vPvZldE.png" alt="Homelab Steps">
<br />
<br />
<br />
First I navigated to Vultr's website and registered for a new account. Once doing so I was given $300 worth of credit for free to use for my project. The first step in the project was to initialize the VMs, so I went to the products tab and clicked deploy server: <br/><br />
<img src="https://i.imgur.com/wxVl0SU.png" alt="Homelab Steps">
<br />
<br />
<br />
These are the specifications that I used for the Domain Client VM. The DC does not need to be extremely over the top with specifications so I just gave it 4 GB of memory and 80 GB of storage: <br/><br />
<img src="https://i.imgur.com/jpKUmlh.png" alt="Homelab Steps">
<br />
<br />
<br />
These are the specifications that I used for the Client VM. With the client being the least sophisticated of the four VMs, I decided to go with a smaller size of 2 GB memory and 55 GB storage: <br/><br />
<img src="https://i.imgur.com/JsgRKUz.png" alt="Homelab Steps">
<br />
<br />
<br />
These are the specifications that I used for the Splunk VM. With the Splunk VM I decided to give 8 GB of memory and 160 GB of storage. Since all of the logs flow through this VM I decided it would be best to have bigger specifications than the other VMs: <br/><br />
<img src="https://i.imgur.com/Y9FMSOn.png" alt="Homelab Steps">
<br />
<br />
<br />
These are the specifications that I used for the Wazuh/Suricata VM. With this VM, I decided to go with 4 GB of memory and 100 GB of storage. This reasoning comes from the fact that it does not need beefy specifications like the Splunk VM but still needs decent memory and storage to function: <br/><br />
<img src="https://i.imgur.com/tdbiygo.png" alt="Homelab Steps">
<br />
<br />
<br />
Going back to the product page, I was now able to see all four VMs had initilized. From here I navigated to the Firewall tab which was under the Network tab: <br/><br />
<img src="https://i.imgur.com/1oYINpt.png" alt="Homelab Steps">
<br />
<br />
<br />
Here I wanted to create a Firewall Group that I could apply to all four VMs. I started by clicking add firewall group: <br/><br />
<img src="https://i.imgur.com/ypHVddz.png" alt="Homelab Steps">
<br />
<br />
<br />
After naming the group, I added all of these inbound rules. In short, port 22 allows me to SSH into any of the machines, port 389 will allow Active Directory to communicate with Shuffle, ports 1514 and 1515 will allow the Wazuh agent to communicate to the Wazuh VM, port 3389 allows for Remote Desktop Connection into any VM, port 8000 allows for Splunk to display the GUI, and port 9997 is the port for Splunk indexing and recieving: <br/><br />
<img src="https://i.imgur.com/XhTcBB3.png" alt="Homelab Steps">
<br />
<br />
<br />
Looking at the settings for the Domain Controller, under the firewall tab, I was able to apply the new firewall to the VM: <br/><br />
<img src="https://i.imgur.com/VnT4YHp.png" alt="Homelab Steps">
<br />
<br />
<br />
<br />
Then I got message letting me know that the firewall group was updated. I then repeated this process for all four VMs: <br/><br />
<img src="https://i.imgur.com/Y2FcsIE.png" alt="Homelab Steps">
<br />
<br />
<br />
Next I set up a VPC Network. This essentially creates a Private IP for the machines to communicate with each other. Similar to before, in the settings, I could click VPC network and then click enable VPC: <br/><br />
<img src="https://i.imgur.com/I1gs3dH.png" alt="Homelab Steps">
<br />
<br />
<br />
This would generate the private IP. I then repeated this for all four VMs: <br/><br />
<img src="https://i.imgur.com/j7KhJ8i.png" alt="Homelab Steps">
<br />
<br />
<br /> 
After doing so, I took note of all the IPs so far, public and private, since I would be referencing them later on: <br/><br />
<img src="https://i.imgur.com/fi6CKLB.png" alt="Homelab Steps">
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
To fix this, I went into the Network Status in the settings. From there I clicked change adapter options, right clicked ethernet instance 0.2, clicked properties, double clicked on IPv4, and manually had it use the private IP I had previously created: <br/><br />
<img src="https://i.imgur.com/3W5G5ma.png" alt="Homelab Steps">
<br />
<br />
<br />
Running the ipconfig command again showed the changes had correctly been applied. I did this same process on the Domain Controller VM as it had the same problem: <br/><br />
<img src="https://i.imgur.com/2i9eu0n.png" alt="Homelab Steps">
<br />
<br />
<br />
Next I used SSH to get into the Splunk VM from my command prompt: <br/><br />
<img src="https://i.imgur.com/HcNyM2p.png" alt="Homelab Steps">
<br />
<br />
<br />
Here I pinged the DC, Client, and Wazuh/Suricata VMs to ensure they could communicate with each other: <br/><br />
<img src="https://i.imgur.com/gnjGpFi.png" alt="Homelab Steps">
<br />
<br />
<br />
This was all for initializing the VMs. The project documentation is continued in the Active Directory Steps: <br/><br />
 https://github.com/rdooley2/Homelab/blob/main/ActiveDirectory.md

