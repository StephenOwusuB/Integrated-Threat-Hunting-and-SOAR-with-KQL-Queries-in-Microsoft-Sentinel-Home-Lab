<h1>Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab</h1>

<h2>Description</h2>
<b>This lab is designed to provide a hands-on experience with integrated threat hunting and Security Orchestration, Automation, and Response (SOAR) using Kusto Query Language (KQL) queries in Microsoft Sentinel. The primary focus of this lab is to equip you with the skills needed to effectively monitor, detect, and respond to security threats within a simulated home lab environment.<b/>
<br />

<h2>Utilities Used</h2>

- <b>Windows Server 2022</b> 
- <b>Microsoft Sentinel (SIEM)</b> 
- <b>Log Analytic Workbooks</b>
- <b>Microsoft Defender for Cloud</b>
- <b>Remote Desktop</b>
- <b>PowerShell</b>
- <b>API's</b>



<h2>Environments Used </h2>

- <b>Microsoft Azure</b>
- <b>Windows 10</b> (21H2)
- <b>Windows Server 2022
- <b>Virtual box</b>

<h2>Links</h2>

- <b>Microsoft Azure Free Trial:</b> https://azure.microsoft.com/en-us/free/
- <b>Windows 10:<b/> https://www.microsoft.com/en-us/software-download/windows10?msockid=3161182fb86f6b5704bc0d55b9fb6aae
- <b>Windows Server 2022:</b> https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022?msockid=3161182fb86f6b5704bc0d55b9fb6aae
- <b>Virtualbox:</b> https://www.virtualbox.org/wiki/Downloads
- <b>Pulsedive:</b> https://pulsedive.com/
  

<h2 align="center">Project walk-through</h2>

<p align="center">
<b>The first thing I am going to do is create a Microsoft Azure account, this will be the cloud environment I'll use to provision my resources. I will take advantage of the $200 credit I'll receive to do this project. The resources I'm using are not very resource heavy, so my credit can be used towards future projects. In addition, I signed up to Pulsedive for free. </b> <br/>
</p>

![Create_Azure_Subscription](https://user-images.githubusercontent.com/108043108/225348757-c41744df-2be1-4ffc-87a6-aa258a4102ef.JPG)
![Set_Up_Completed](https://user-images.githubusercontent.com/108043108/225348950-5d9c01fa-a707-4813-a6f3-012c703c41df.JPG)
![Create Pulsedive account](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/pulsedive.png)

<p align="center">
<b>Next, I downloaded VirtualBox onto my local laptop to facilitate the running of virtual machines. Subsequently, I obtained both Windows Server 2022 and Windows 10 for this lab at no cost. I proceeded to configure the Windows Server 2022 with Active Directory, DNS, and DHCP. Additionally, I joined the Windows 10 PC to the domain. While the steps to configure these Windows Server services and Windows 10 are beyond the scope of this lab, further details can be found in the associated lab documentation [Securing Windows Server and Active Directory: A Comprehensive Home Lab Project](https://github.com/YourUsername/Securing-Windows-Server-and-AD) </b> <br/>
</p>

![Download virtualbox](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/virtualbox%20dw.png)
![Download Windows Server 2022](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/server2022.png)
![Download Windows 10](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/win10%20dw.png)


<br />
<br />
<p align="center">
<b>The next thing I'll do is create a resource group in azure. It's essential to create a new Resource Group to house all future resources. In Azure, a Resource Group serves as a logical container for tools, services, configurations, and more, allowing them to be managed collectively. This means they can be created or deleted simultaneously, sharing the same lifecycle. If resources are placed outside of a specific Resource Group, they will persist even if the Resource Group is deleted. Grouping resources in this way simplifies their management. Ensure you are using the correct account associated with your subscription to avoid encountering errors when creating resource groups. Search for 'Resource Group' on the Azure dashboard and click on 'Create' to begin the process. Name the resource group 'Azure Sentinel Training Lab RG', set the region to your region in my case I will set it to 'East US', then click 'Review + create' and 'Create' to validate and create the resource group.</b> <br/>
</p>

![Create Resource Group](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/deploy%20RG%201.png)
![Setup Complete for Resource Group](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/deploy%20RG%202.png)

<p align="center">
<b>Return to the Azure main page and search for "Log Analytics Workspace". Click on "Create". Specify the subscription (in this case, Azure subscription 1). Select your resource group. Name your Log Analytics Workspace (for example, sentinel-training-lab-ws). Choose the same location as the resource group (East US). You will see "Validation passed". Click on "Create". You will see "Deployment was successful".</b> <br/>
</p>

![Create Log Analytics Workspace](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/Log%20analytics%20workspaces%201.png)
![Setup Log Analytics Workspace](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/log%20analytics%20workspaces%202.png)
![Setup Log Analytics Workspace](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/log%20analytics%20workspace%203.png)
![Setup Complete for Log Analytics Workspace](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/log%20analytics%20workspace%204.png)


<p align="center">
<b>Return to the Azure main page and search for "Microsoft Sentinel". Click on "Create Microsoft Sentinel" or simply "Create". When setting up Microsoft Sentinel for the first time, you receive a free trial for 31 days. Select the Log Analytics workspace you created and click on "Add". This action will add the workspace to Microsoft Sentinel. Once Azure completes the addition process, you will see a notification stating "Successfully added Microsoft Sentinel."</b> <br/>
</p>

![Create Sentinel](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%201.png)
![Setup Sentinel](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%202.png)
![Setup Complete Sentinel](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%203.png)

<p align="center">
<b>Next, I will set up the Microsoft Training Lab solution. Unfortunately, I couldn't find it in the content management. Instead, I installed other services such as Azure Activity, Azure Entra ID, and Threat Intelligence TAXII. Installing the data connector involves two steps: first, install the connector, and second, connect the connector to Sentinel. To install Azure Activity, select Content management in Microsoft Sentinel. Click on Content hub. Search for Azure Activity and click on Install. I noticed an issue with the Azure data connector sometimes when you connect it, it sits there spinning. To connect the Azure Activity data connector: On the left tab, scroll down to Data connectors and select Azure Activity. Click on Open connector page. Go to the Resource group section and click on the resource group. Select Activity log. Click on Export activity log. Click on Add diagnostics. Specify the diagnostics name (e.g., Azure Activity). Check all options except for Archive, Stream, and Send to partner solution. Click on Save.

![Install Azure Activity](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%205.png)
![Setup Azure Acitivity](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%206.png)
![Setup Azure Acitivity](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%207.png)
![Setup Azure Acitivity](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%208.png)
![Setup Azure Acitivity](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%209.png)
![Setup Azure Acitivity](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2010.png)
![Setup Azure Acitivity](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2011.png)
![Setup Azure Acitivity](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2012.png)
![Setup Azure Acitivity](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2013.png)
![Setup Azure Acitivity](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2014.png)
![Setup Azure Acitivity](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2015.png)
![Setup Azure Acitivity](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2016.png)
![Setup Azure Acitivity](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2017.png)

</b> <br/>
</p>

<p align="center">
<b>To install the Entra ID connector, navigate to Sentinel and select your workspace. Scroll down to the Content hub and search for Entra ID. Click on Install. Once the connector is installed, we will need to configure a few settings. It may take 5 to 10 minutes for the connector to show as connected. Scroll to Data connectors and click on Open connector page. Check the following options: Sign-in logs, Audit logs, Non-interactive user sign-ins log, Service principal sign-in log, Provisioning logs, User risk events, and Risky users. Click on Apply changes. Navigate back to the Data connectors page and click on Refresh.</b> <br/>
</p>

![Install Azure Entra ID](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2018.png)
![Setup Azure Entra ID](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2019.png)
![Setup Azure Entra ID](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2020.png)


<p align="center">
<b>To install the Threat Connector into Sentinel, go to the Content hub and search for "Threat Intelligence". Navigate to the Data connector and select "Threat Intelligence TAXII". Visit Pulsedive, which can be integrated into Sentinel and Myst. On the Pulsedive page, select the API tab at the top, then select "Indicators". Scroll down to find the API root URL, API key, and Collection IDs. Copy the API root URL, username, and API key from this page, and paste them into Sentinel. Note that the password is the API key. Next, select "Collections" on the left tab and copy the test collection. Click on "Add" to add Pulsedive. In a few minutes, you will see the data from the connector.</b> <br/>

![Install Threat Intelligence](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2021.png)
![Copy API keys from Pulsedive](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2022.png)
![Copy Root URL from Pulsedive](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2024.png)
![Setup Threat Intelligence](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2025.png)
![Complete Setup Threat Intelligence](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2026.png)
  
</p>


<p align="center">
<b>Next we will create a set of rules that will enhance our detection and hunting of threats. Creating KQL queries, deploying NRT, ML, Fusion, Microsoft Sentinel UEBA (User Entity Behavior Analytics), and automated rules involves several steps. Use cases include malicious insider, compromised user, APT, zero-day, and known threats. Analytics techniques such as supervised machine learning, unsupervised machine learning, statistical modeling, and rule-based systems are employed. Data sources encompass events and logs, network flows and packets, business context, HR and user context, and external threat intelligence. To enable UEBA in Sentinel, go to Sentinel, select Entity Behavior, then Entity Behavior Settings, and turn on the UEBA feature. Select Microsoft Entra ID, apply it, and choose the existing data sources for entity behavior analytics (Audit logs, Azure Activity, Signing logs). Ensure to sync Microsoft Sentinel with at least one directory service to avoid validation errors. For anomalies, go to Sentinel, scroll to Analytics, select Anomalies, apply a filter, and choose a data source (e.g., Entra ID). To create a schedule query rule in Sentinel, go to Sentinel, select Analytics, click Create, and choose Schedule Query Rule. Specify the rule's name, description, severity, and MITRE ATT&CK (default), ensuring status is enabled. Add your query (e.g., Auditlogs | where OperationName == "Add user"), schedule it to run every 5 minutes, and set the alert threshold to 0. Enable incident settings, review, create, and save the query. For NRT setup, follow similar steps to create an NRT Query Rule. To configure ML rules, go to Sentinel, select Analytics, then Templates, filter for ML behavior, and configure RDP and SSH rules. Note that ML rules take 7 days to show. For automation rules, go to Sentinel, select Automation, create an Automation Rule, and specify the rule's details and actions (e.g., change status to active, set severity to high, assign an owner, add tags). Schedule rule expiration and save the rule. Note that the order of rule execution matters, and it takes a few minutes for new user data to appear in the portal. Custom rules can be duplicated, edited, and tested in flight mode before putting them into production.</b> <br/>

![Setup Fusion rule](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2027.png)
![InitialSetup of scheduled query rule](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2028.png)
![Setup of scheduled query rule](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2029.png)
![Setup scheduled query rule](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2030.png)
![Setup scheduled query rule](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2031.png)
![Complete setup of scheduled query rule](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2032.png)
![Sheduled rule showing in list of rules](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2033.png)
![Setup ML Behavior Analytics Rule](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2034.png)
![Setup Automated rule](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2035.png)
![Setup Automated rule](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2036.png)
![Setup Automated rule](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2037.png)
![Setup Watchlist](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2044.png)
![Setup Watchlist](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2045.png)
![Setup Watchlist](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2046.png)
![Setup Watchlist](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2047.png)
![Setup Watchlist](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2048.png)
![Setup High count of connections by client](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2049.png)
![Setup High count of connections by client](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2050.png)
![Complete setup of High count of connections by client](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2051.png)
![Setup UEBA](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2081.png)
![Setup UEBA](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2057.png)
![Setup UEBA](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2058.png)
![Setup UEBA](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2059.png)
![Setup UEBA](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2060.png)
![Setup UEBA](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2061.png)
![Setup Entity Behaviour](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2078.png)
![Setup Entity Behaviour](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2079.png)
![Setup Entity Behaviour](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2080.png)
![List of rules](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2052.png)


</p>

<p align="center">
<b>To threat hunt you will have to set up a new hunt in Sentinel, go to Sentinel, select Hunting, click on Create a new hunt, specify the hunt name, assign ownership to a user (e.g., your default account), leave the rest of the default settings, and click on Create. The hunt will appear, and you should click on it. Next, create queries to monitor Entra ID accounts by clicking on Query Actions, selecting New Query, specifying the query name and description, and entering the query (e.g., AuditLogs | where OperationName == "Add user"). Select relevant tactics and techniques, typically related to persistence for account creation, and click Create. For a second query to monitor deleted users, click on Query Actions, select New Query, specify the name (e.g., Entra ID Audit Log-Deleted User), click on Valid Accounts, add Cloud Account, check Defense Evasion, and save the hunt..</b> <br/>

![Create Hunting rules](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2062.png)
![Create Hunting rules](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2063.png)
![Create Hunting rules](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2064.png)
![Create Hunting rules](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2065.png)
![View Hunting Queries](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2068.png)
</p>

<p align="center">
<b>To test the queries, first create a user in Active Directory on your local server. Then, force synchronization to Azure using the PowerShell script:Import-Module ADSync, Start-ADSyncSyncCycle -PolicyType Delta. Next, create a user in Azure Entra ID and delete the user.</b> <br/>
  
![Create User in AD](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2038.png)
![Create User in AD](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2039.png)
![Create User in AD](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2040.png)
![Synced User to Entra ID](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2041.png)
![Created User in Entra ID](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2066.png)
![View User in Entra ID](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2067.png)
![Queries](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2069.png)
![Queries](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2070.png)
![Logs](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2053.png)
![Logs](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2054.png)

</p>

<p align="center">
<b>To automate ticket creation for incidents based on the established queries, create an incident rule.</b> <br/>
  
  ![Create Incident rule](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2071.png)
  ![Create Incident rule](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2072.png)
  ![Create Incident rule](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2073.png)
  ![Create Incident rule](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2074.png)
  
</p>

<p align="center"> 
<b>Next, review the incident by navigating to the Incidents section in Sentinel. Here, you will find details such as severity, incident number, title, alerts, and incident provider. Select the incident to view a popup on the right with options to change the status, severity, and view full details. When you click on "View full details," you will see the incident timeline, entities, top insights, similar incidents, and evidence.</b> <br/>

  ![View Incident](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2076.png)
  ![View Full Details](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/sentinel%2077.png)
  
</p>

## Skills Learned

- **Understanding KQL**: Develop proficiency in crafting and executing KQL queries to analyze and interpret data.
- **Threat Detection**: Learn how to identify and respond to potential security threats using advanced detection techniques.
- **Incident Response**: Gain experience in managing and mitigating security incidents through automated and manual response strategies.
- **Setting Up and Configuring Sentinel**: Understand the process of setting up Microsoft Sentinel, creating rules, and configuring alerts.
- **Leveraging UEBA**: Learn how to enable and utilize User Entity Behavior Analytics (UEBA) to detect anomalies and suspicious activities.
- **Automation and SOAR**: Explore the capabilities of SOAR to streamline and automate incident response processes.
- **Effective Monitoring**: Improve your ability to continuously monitor network and system activities for potential threats.
- **Hands-on Practice**: Apply theoretical knowledge in a practical, simulated environment to reinforce learning and skill development.

## Acknowledgments

Special thanks to all contributors and the community for their support and guidance. This project was inspired by various resources and projects in the cybersecurity and IT fields.




