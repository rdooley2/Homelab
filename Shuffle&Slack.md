<p align="center">
The last technical part of the project revolved around alert automation in Shuffle and Slack. My plan was that whenever an alert triggered, the three from previous steps, a message would be sent out in Slack. This would simulate automation and what you might see in a work enviornment. First, I navigated to Shuffle and created an account. When brought to the home page, I clicked create workflow:<br/><br />
<img src="https://i.imgur.com/pIznaqL.png" alt="Homelab Steps">
<br />
<br />
<br />
This workflow is where all of the automation happens. After naming the workflow, I grabbed a Webhook and dragged it onto the board. I named it appropriately and then copied the URL it provided:<br/><br />
<img src="https://i.imgur.com/LXRfSTC.png" alt="Homelab Steps">
<br />
<br />
<br />
Inside of Splunk Enterprise, I navigated to my alerts and edited the File Change Alert. Additionally, I had already come in and disabled all of the alerts beforehand, with the intention of enabling them once everything was set up correctly:<br/><br />
<img src="https://i.imgur.com/xV99bU5.png" alt="Homelab Steps">
<br />
<br />
<br />
Once viewing the alert settings, I first enabled throttling. This prevents overwhelming the Shuffle server and getting rate-limited in the case that there is an event spike. Secondly, I added a webhook under the actions section and pasted in the URL from before. This link tells Splunk to send the alert JSON to Shuffle so that it can automate a response using the data:<br/><br />
<img src="https://i.imgur.com/ddF41ip.png" alt="Homelab Steps">
<br />
<br />
<br />
Next I went into Slack and created a new account. After doing so, I created a new workspace. This would help to simulate a work enviornment chat:<br/><br />
<img src="https://i.imgur.com/H8sbR84.png" alt="Homelab Steps">
<br />
<br />
<br />
Once naming the chat and running through all of the welcome prompts, I was greeted with my new Slack Channel:<br/><br />
<img src="https://i.imgur.com/MTLrogN.png" alt="Homelab Steps">
<br />
<br />
<br />
Back in the Shuffle workflow, I dragged the Slack app over to begin setting up the actual alert. At this point I authenticated Slack by clicking authenticate underneath the name, but in this screenshot I had already done so:<br/><br />
<img src="https://i.imgur.com/W5ODe2T.png" alt="Homelab Steps">
<br />
<br />
<br />
In Shuffle, I created a new channel and named it alerts. All automated alerts would be sent out in this channel. After doing so, I grabbed the channel ID from the URL:<br/><br />
<img src="https://i.imgur.com/q3MSbgt.png" alt="Homelab Steps">
<br />
<br />
<br />
Back in Shuffle, I pasted over the channel ID. I also went ahead and connected the webhook to the Slack block. This essentially makes it so that when the webhook recieves event data, it will then send out an event through Slack using that data:<br/><br />
<img src="https://i.imgur.com/1YZC7rm.png" alt="Homelab Steps">
<br />
<br />
<br />
With all of that done, I needed to enable the alert in Splunk Enterprise. To create the automated text in Shuffle, I first would need to ensure that alert data was being sent to Shuffle:<br/><br />
<img src="https://i.imgur.com/uKSCYse.png" alt="Homelab Steps">
<br />
<br />
<br />
In the Client, I quickly added a new file in the downloads folder and navigated back to Splunk Enterprise to ensure that the alert was triggering:<br/><br />
<img src="https://i.imgur.com/067PMBo.png" alt="Homelab Steps">
<br />
<br />
<br />
Within the Workflow runs section, I was able to confirm that Shuffle was recieving the data and sending alerts through Slack:<br/><br />
<img src="https://i.imgur.com/qcY7Ouu.png" alt="Homelab Steps">
<br />
<br />
<br />
Next I needed to write the message in Shuffle using the data from Splunk Enterprise. For this alert, I displayed the user, machine, and time of alert:<br/><br />
<img src="https://i.imgur.com/ffFp8LC.png" alt="Homelab Steps">
<br />
<br />
<br />
I repeated this process for the other two alerts and my workflow ended up looking like this:<br/><br />
<img src="https://i.imgur.com/b5ScN4n.png" alt="Homelab Steps">
<br />
<br />
<br />
Finally, here are the results in the Slack alerts channel (You may notice the pfp and name are different in this screenshot because I changed them) :<br/><br />
<img src="https://i.imgur.com/CJfq6Ev.png" alt="Homelab Steps">
<br />
<br />
<br />
This was all for setting up automation in Shuffle and Slack. The project documentation is continued in the Dashboard Steps: <br/><br />
https://github.com/rdooley2/Homelab/blob/main/Dashboard.md
