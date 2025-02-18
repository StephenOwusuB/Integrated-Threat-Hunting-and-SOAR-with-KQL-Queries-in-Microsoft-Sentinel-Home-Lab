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
<b>The first thing I am going to do is create a Microsoft Azure account, this will be the cloud environment I'll use to provision my resources. I will take advantage of the $200 credit I'll receive to do this project. The resources I'm using are not very resource heavy, so my credit can be used towards future projects. Also included is the website I will be using for my IP geolocation data. </b> <br/>
</p>

![Create_Azure_Subscription](https://user-images.githubusercontent.com/108043108/225348757-c41744df-2be1-4ffc-87a6-aa258a4102ef.JPG)
![Set_Up_Completed](https://user-images.githubusercontent.com/108043108/225348950-5d9c01fa-a707-4813-a6f3-012c703c41df.JPG)
![Download virtualbox](https://user-images.githubusercontent.com/108043108/225348950-5d9c01fa-a707-4813-a6f3-012c703c41df.JPG)
![Download Windows Server 2022](https://user-images.githubusercontent.com/108043108/225348950-5d9c01fa-a707-4813-a6f3-012c703c41df.JPG)
![Download Windows 10](https://user-images.githubusercontent.com/108043108/225348950-5d9c01fa-a707-4813-a6f3-012c703c41df.JPG)
![Create Pulsedive account](https://github.com/StephenOwusuB/Integrated-Threat-Hunting-and-SOAR-with-KQL-Queries-in-Microsoft-Sentinel-Home-Lab/blob/main/images/images/pulsedive.png)

<br />
<br />
<p align="center">
<b>The next thing I'll do is start the process of creating my virtual machines. Provisioning a VM can be a lengthy process so while I move on to the next step, my VM can be provisioned in the background.</b> <br/>
</p>
