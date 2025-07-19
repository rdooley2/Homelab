<h2>Project Index</h2>

- <b>[Active Directory Steps](https://github.com/rdooley2/Homelab/blob/main/ActiveDirectory.md)</b>
- <b>[Network Drive Steps](https://github.com/rdooley2/Homelab/blob/main/NetworkDrive.md)</b>
- <b>[Splunk Steps](https://github.com/rdooley2/Homelab/blob/main/Splunk.md)</b>
- <b>[Wazuh Steps](https://github.com/rdooley2/Homelab/blob/main/Wazuh.md)</b>
- <b>[Suricata Steps](https://github.com/rdooley2/Homelab/blob/main/Suricata.md)</b>
- <b>[Shuffle & Slack Steps](https://github.com/rdooley2/Homelab/blob/main/Shuffle&Slack.md)</b>
- <b>[Dashboard Steps](https://github.com/rdooley2/Homelab/blob/main/Dashboard.md)</b><br>

<h2>Network Drive Steps</h2>
<p align="center">
The next step of the project was to set up a few Network Drives. These are essentially shared folders and can be useful for any work environment where files need to be easily accessible. First, I went into the DC VM: <br/><br />
<img src="https://i.imgur.com/58rEzxr.png" alt="Homelab Steps">
<br />
<br />
<br />
Within the C: Drive, I created three folders, one for each group. For each folder, I right-clicked the folder, clicked properties, advanced sharing, permissions, add user, advanced, searched for the respective group, and clicked okay. This sets up the network path for file sharing, which was needed in a later step: <br/><br />
<img src="https://i.imgur.com/vGmiAtm.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I went into the Group Policy Management application and created a GPO in the RA group's Organizational Unit: <br/><br />
<img src="https://i.imgur.com/tzsnYog.png" alt="Homelab Steps">
<br />
<br />
<br />
First, I had to right click and select edit on the GPO: <br/><br />
<img src="https://i.imgur.com/uXqxe1e.png" alt="Homelab Steps">
<br />
<br />
<br />
Then I navigated to the Drive Maps section, right clicked, and selected new mapped drive: <br/><br />
<img src="https://i.imgur.com/962a9HW.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I went back to the folder and grabbed the network path that I created in a previous step: <br/><br />
<img src="https://i.imgur.com/RHKN79G.png" alt="Homelab Steps">
<br />
<br />
<br />
I used this network path in the new mapped drive so it knew where to look. I then clicked okay: <br/><br />
<img src="https://i.imgur.com/5RDNSO6.png" alt="Homelab Steps">
<br />
<br />
<br />
Logging into one of the users from the RA group on the Client VM, I was able to confirm the drive had been mapped correctly: <br/><br />
<img src="https://i.imgur.com/DLZciEq.png" alt="Homelab Steps">
<br />
<br />
<br />
I then created a test file in the drive: <br/><br />
<img src="https://i.imgur.com/JHLU9ym.png" alt="Homelab Steps">
<br />
<br />
<br />
Logging into another member of the RA group, I saw the same file was there. I repeated this process for the other two groups: <br/><br />
<img src="https://i.imgur.com/VqCfKlG.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I wanted to create a shared folder for all users. Navigating back to the DC, I created a folder in the C: Drive and set it up similar to before. The only difference is I had it shared to all authenticated users instead of just one group: <br/><br />
<img src="https://i.imgur.com/cEdRnxD.png" alt="Homelab Steps">
<br />
<br />
<br />
Going into Group Policy Management, I created a new GPO within "Dooley.local" that would target all users. Setting up the mapped drive was the same process as before, making it easy to set up: <br/><br />
<img src="https://i.imgur.com/Q9kYNCD.png" alt="Homelab Steps">
<br />
<br />
<br />
To test the new folder, I logged in as a user from each group. Here is the GHA view: <br/><br />
<img src="https://i.imgur.com/u8FuvAG.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, here is the REC view: <br/><br />
<img src="https://i.imgur.com/ceRCjkX.png" alt="Homelab Steps">
<br />
<br />
<br />
Finally, here is the RA view: <br/><br />
<img src="https://i.imgur.com/mL55pOi.png" alt="Homelab Steps">
<br />
<br />
<br />
This was all for setting up the Network Drives. The project documentation is continued in the Splunk Steps: <br/><br />
https://github.com/rdooley2/Homelab/blob/main/Splunk.md
