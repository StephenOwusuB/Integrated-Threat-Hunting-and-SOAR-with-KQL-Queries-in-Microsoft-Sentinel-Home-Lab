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

