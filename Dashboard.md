<p align="center">
The final part of the project revolved around creating a personal dashboard in Splunk. To start, I navigated to Wazuh's official documentation page and downloaded the Security Events and Vulnerabilities Dashboards. Using this code would provide a strong foundation for the Dashboard:<br/><br />
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
After applying the changes, this is what the dashboard looked like. I should note that the vulnerability alert and alert severity sections are empty because the VMs currently have no detected vulnerabilities. The only exception, as seen in the alert severity section, is where vulnerabilities were detected by Suricata but not assigned a severity rating:<br/><br />
<img src="https://i.imgur.com/TdblzQs.png" alt="Homelab Steps">
<img src="https://i.imgur.com/nCtqSb8.png" alt="Homelab Steps">
<br />
<br />
<br />
:<br/><br />
<img src="" alt="Homelab Steps">
<br />
<br />
<br />
:<br/><br />
<img src="" alt="Homelab Steps">
<br />
<br />
<br />
:<br/><br />
<img src="" alt="Homelab Steps">
<br />
<br />
<br />
Thanks for reading this far! This is the end of the project. This was the most challenging but rewarding project that I have ever taken on. I learned a lot and I hope you were able to as well!
