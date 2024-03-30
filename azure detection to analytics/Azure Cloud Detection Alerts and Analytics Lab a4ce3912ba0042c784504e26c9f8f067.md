# Azure Cloud Detection Alerts and Analytics Lab

---

# **Overview**

As a cyber security student on the path to becoming a Cloud Security Architect, detection and analysts jobs are often the starting point. SIEM’s or Security Information and Event Management Systems help with this. This tool allows SOC analysts to analyze log data from a variety of sources within their network such as Firewalls, IDS/IPS, Identity solutions, and many more tasks.

This lab uses Microsoft Sentinel as a SIEM solution to provide security professionals with security analytics, threat intelligence, threat response, and other services in a single platform.

# **In this Lab I will**

- Understand Windows Security Event logs
- Configure Windows Security Policies
- Utilize KQL to query Logs
- Write Custom Analytic Rules to detect Microsoft Security Events logging Incidents for Incident Response

NOTE:

When not in use please make sure you turn off your Virtual Machines and when the lab is complete, delete any resources and services that are not free to avoid incurring any unnecessary cost.

This lab will use an environment in Azure that I use for testing Azure resources Cloud.

Network Topology

![Untitled](Azure%20Cloud%20Detection%20Alerts%20and%20Analytics%20Lab%20a4ce3912ba0042c784504e26c9f8f067/Untitled.png)

# **Part 1: Generating Security Events**

Simply logging on to a virtual machine via creates an event in Event viewer. I’m going to find this event in the event viewer and make it so every time a user logs on, an alert is created and sent to my incidents in Sentinel.

Every event has what is known as an event ID or EID. The EID for a successful logon attempt is 4624.

You can see this by going to Logs in Microsoft Sentinel and creating a new query. Then, type in

```html
SecurityEvent
| where EventID == 4624  // Event ID for successful logon
```

![Untitled](Azure%20Cloud%20Detection%20Alerts%20and%20Analytics%20Lab%20a4ce3912ba0042c784504e26c9f8f067/Untitled%201.png)

# **Part 2: Writing Analytic Rules and Generating an Alert**

We will now write an analytic rule to show when a user logs on successfully.

For this I will search for Microsoft Sentinel and navigate to Analytics. Next click on “+ Create” button and “Scheduled query rule”. In the General tab, I will give the rule the following settings.

![Untitled](Azure%20Cloud%20Detection%20Alerts%20and%20Analytics%20Lab%20a4ce3912ba0042c784504e26c9f8f067/Untitled%202.png)

Next, in the Set rule logic tab I will use the query I used earlier to create the alert.

![Untitled](Azure%20Cloud%20Detection%20Alerts%20and%20Analytics%20Lab%20a4ce3912ba0042c784504e26c9f8f067/Untitled%203.png)

Make sure to use these Entity mapping settings as well so we can identify where the alert is coming from for further analysis if needed.

![Untitled](Azure%20Cloud%20Detection%20Alerts%20and%20Analytics%20Lab%20a4ce3912ba0042c784504e26c9f8f067/Untitled%204.png)

Continue to the “Review + Create” tab then click save.

Here in analytics, we can see that the rule has been added to our set of rules.

![Untitled](Azure%20Cloud%20Detection%20Alerts%20and%20Analytics%20Lab%20a4ce3912ba0042c784504e26c9f8f067/Untitled%205.png)

# **Part 3: Observe the Alerts in Microsoft Sentinel**

Here are the initial alerts in Microsoft Sentinel Incidents before enabling them to be logged. Bear in mind that the Virtual Machines time is 5 hours ahead of the Host Machines time. So the VM would read 1:03 AM for its time, not 8:03 PM (My host machine’s time).

![Untitled](Azure%20Cloud%20Detection%20Alerts%20and%20Analytics%20Lab%20a4ce3912ba0042c784504e26c9f8f067/Untitled%206.png)

![Untitled](Azure%20Cloud%20Detection%20Alerts%20and%20Analytics%20Lab%20a4ce3912ba0042c784504e26c9f8f067/Untitled%207.png)

![Untitled](Azure%20Cloud%20Detection%20Alerts%20and%20Analytics%20Lab%20a4ce3912ba0042c784504e26c9f8f067/Untitled%208.png)

In order to be able to see these events in Microsoft Sentinel Incidents we have to enable it. You could enable it via event viewer, but I prefer to use the command prompt. It is always good to familiarize yourself with commands you will need to use on the job (and it makes you look cool in front of your friends).

Open the command prompt as administrator and type the following command to enable these logs

```
Auditpol /set /Category:System /success:enable
```

![Untitled](Azure%20Cloud%20Detection%20Alerts%20and%20Analytics%20Lab%20a4ce3912ba0042c784504e26c9f8f067/Untitled%209.png)

Then close the Virtual Machine and wait 10 minutes for the changes to go through. After this, logon to the VM and navigate to Microsoft Sentinel Incidents.

Now refresh incidents to view the update logs. It will then show the logon success alert we created. This includes the date and time as well.

![Untitled](Azure%20Cloud%20Detection%20Alerts%20and%20Analytics%20Lab%20a4ce3912ba0042c784504e26c9f8f067/Untitled%2010.png)

Click on this incident and View full details to investigate it further showing the workspace and even the VM name from the entities we added in our rule.

![Untitled](Azure%20Cloud%20Detection%20Alerts%20and%20Analytics%20Lab%20a4ce3912ba0042c784504e26c9f8f067/Untitled%2011.png)

![Untitled](Azure%20Cloud%20Detection%20Alerts%20and%20Analytics%20Lab%20a4ce3912ba0042c784504e26c9f8f067/Untitled%2012.png)

![Untitled](Azure%20Cloud%20Detection%20Alerts%20and%20Analytics%20Lab%20a4ce3912ba0042c784504e26c9f8f067/Untitled%2013.png)

This process can be repeated for all Event IDs. There is tons of documentation online about Azure resources and how to go into depths about analyzing incidents and performing incident response.

As always I hope the article proves useful to anyone wanting learn more Azure services. I plan to do more in depths cloud labs in the future. I wish anyone who reads this luck in their pursuit of knowledge of the Cloud.