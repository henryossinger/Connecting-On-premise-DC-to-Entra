# Connecting-On-premise-DC-to-Entra

<h2>Technologies Used</h2>

- Windows Server 2022 
- VMWare Workstation Player
- Active Directory
- Microsoft Entra Connect

<h2>First Step</h2>
One of the first items that needs to be completed for any environment build is the creation of Active Directory. AD provides unified user administration, the ability to push GPOs, and much more. Connecting the DC to Entra takes administrative possibilites a step further; allowing our domain to integrate cloud technologies. Although a very simple step there is almost no better way to begin the creation of our environment. 

<p>
  
I am not going to go into how to setup a Domain Controller and will start with a DC already setup. If you would like to know how to set one up, visit the repository below where I create one. 

https://github.com/henryossinger/Active-Directory

<h2>Prerequisite’s</h2>
Before we start, we will need an Entra tenant to connect our DC. This can be done by creating an Azure account which will automatically create a default Entra tenant. We will need some information from this tenant to be able to connect the DC: 

1. Global Admin login credentials.
2. Domain name for the tenant.

When creating an Azure account, a global admin will also be created by default. To view our global admin and their UPN (needed for logging in), go to 'Users' within Entra. After locating the user, we can verify they are a global admin by looking at the roles assigned to them. 

<img src="https://imgur.com/LdemfDx.png" height="50%" width="50%" alt="Assigned Roles"/>

<h2>Installing Entra Connect</h2>

Now that we have that information, let's connect to our DC and install Entra Connect. Entra Connect is the software used to link the DC to an Entra tenant and start the synchronization. Entra Connect can be downloaded from <a href="https://www.microsoft.com/en-us/download/details.aspx?id=47594">this</a> Microsoft Page.

<img src="https://imgur.com/zhBFu5z.png" height="50%" width="50%" alt="Current Topology"/>
