<h2>Project Index</h2>

- <b>[Virtual Machine Steps](https://github.com/rdooley2/Homelab/blob/main/README.md)</b>
- <b>[Active Directory Steps](https://github.com/rdooley2/Homelab/blob/main/ActiveDirectory.md)</b>
- <b>[Network Drive Steps](https://github.com/rdooley2/Homelab/blob/main/NetworkDrive.md)</b>
- <b>[Splunk Steps](https://github.com/rdooley2/Homelab/blob/main/Splunk.md)</b>
- <b>[Wazuh Steps](https://github.com/rdooley2/Homelab/blob/main/Wazuh.md)</b>
- <b>[Suricata Steps](https://github.com/rdooley2/Homelab/blob/main/Suricata.md)</b>
- <b>[Shuffle & Slack Steps](https://github.com/rdooley2/Homelab/blob/main/Shuffle&Slack.md)</b>
- <b>[Dashboard Steps](https://github.com/rdooley2/Homelab/blob/main/Dashboard.md)</b><br>

<h2>Dashboard Steps</h2>
<p align="center">
The final part of the project revolved around creating a personal dashboard in Splunk. To start, I navigated to Wazuh's official documentation page and downloaded the Security Events and Vulnerabilities Dashboards. Using this code would provide a strong foundation for the dashboard:<br/><br />
<img src="https://i.imgur.com/oORN8vR.png" alt="Homelab Steps">
<br />
<br />
<br />
In Splunk Enterprise, I clicked the Dashboards section:<br/><br />
<img src="https://i.imgur.com/TGk2SPd.png" alt="Homelab Steps">
<br />
<br />
<br />
Once in the dashboard page, I clicked create dashboard in the top right:<br/><br />
<img src="https://i.imgur.com/iOXUXrc.png" alt="Homelab Steps">
<br />
<br />
<br />
I then named the new dashboard, selected dashboard studio, and selected grid for the layout:<br/><br />
<img src="https://i.imgur.com/fIKI8OA.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I clicked the source code button near the top left:<br/><br />
<img src="https://i.imgur.com/3g3FbzM.png" alt="Homelab Steps">
<br />
<br />
<br />
Then, I replaced the default code with the code from both dashboard files that I previously downloaded:<br/><br />
<img src="https://i.imgur.com/zcmyAr1.png" alt="Homelab Steps">
<br />
<br />
<br />
After applying the changes, this is what the dashboard looked like:<br/><br />
<img src="https://i.imgur.com/TdblzQs.png" alt="Homelab Steps">
<img src="https://i.imgur.com/groAOJL.png" alt="Homelab Steps">
<br />
<br />
<br />
Next, I wanted to create some visualizations using the three alerts as a base. First, I created a new alert for failed logons that was very simple. Next, I clicked visualize:<br/><br />
<img src="https://i.imgur.com/LYttRai.png" alt="Homelab Steps">
<br />
<br />
<br />
Splunk then visualized the data as a bar graph. There are many other options for visualization, but I wanted to keep it as a bar graph. To use this on my dashboard, I clicked save as existing dashboard, in the top right and selected my dashboard. This automatically adds the graph to my dashboard:<br/><br />
<img src="https://i.imgur.com/YE6Ean8.png" alt="Homelab Steps">
<br />
<br />
<br />
I repeated this process for the other two alerts. The only difference is that I wanted the other two visualizations to be tables. Here is the table I created for the detected file changes using the alert from before:<br/><br />
<img src="https://i.imgur.com/g625mrm.png" alt="Homelab Steps">
<br />
<br />
<br />
Finally, here is the table I generated for detected scans using the alert from before:<br/><br />
<img src="https://i.imgur.com/tmtEmf1.png" alt="Homelab Steps">
<br />
<br />
<br />
Navigating back to the dashboard, I could see the three visualizations. I have also included the text file with the code for the dashboard in this repository:<br/><br />
<img src="https://i.imgur.com/1k7gqy8.png" alt="Homelab Steps">
<br />
<br />
<br />
Thanks for reading this far! This is the end of the project. This was the most challenging but rewarding project that I have ever taken on. I learned a lot, and I hope you were able to as well!
