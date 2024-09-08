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
Before we start, we will need an Entra tenant to connect our DC. This can be done by creating an Azure account which will automatically create a default Entra tenant. To connect the tenant, we will need the Global Admin login credentials.

When creating an Azure account, a global admin will be created by default. To view our global admin and their UPN (needed for logging in), go to 'Users' within Entra. After locating the user, we can verify they are a global admin by looking at the roles assigned to them. 

<img src="https://imgur.com/LdemfDx.png" height="50%" width="50%" alt="Assigned Roles"/>

<h2>Installing Entra Connect</h2>

Now that we have that information, let's connect to our DC and install Entra Connect. Entra Connect is the software used to link the DC to an Entra tenant and start the synchronization. Entra Connect can be downloaded from <a href="https://www.microsoft.com/en-us/download/details.aspx?id=47594">this</a> Microsoft Page.

Once Downloaded, we will follow the very simple prompts asking us to login to the Global Admin for the Entra tenant, and the domain admin for the DC. After this we will be asked how we want our users to be able to sign in. Microsoft recommends 'Password Hash Synchronization' as the default, so we will go with that. Password hash synchronization syncs the on-prem user passwords with the Entra passwords, and vice versa. This allows users to login with the same credentials across all services that use their Microsoft account, and ensure that the passwords match up in both locations. The hashing portion is a secuity feature that makes it far more difficult to use said password, even if the hash is stolen. 

Once that is complete, Entra connect will begin downloading the synchronization tools and configuring the connect between the cloud and the on-prem server. 

<img src="https://imgur.com/DBt28Xc.png" height="50%" width="50%" alt="Assigned Roles"/>

<h2>Errors when setting up Entra Connect</h2>

When I was running through this, it was at this point that I began receiving errors from Entra Connect, stating the configuration was failing. Initially I retried the connect, uninstalled and re-installed Entra Connect, and restarted the server to see if that would resolve the issue. After none of these worked I decided to check the log file that was mentioned in the error I was receiving (picture below). When reviewing the log file I noticed that every single time I attempted the configuration process it would error out at the same time, which was when it was attempting to setup the synchronization service. 

<img src="https://imgur.com/WpIGtse.png" height="50%" width="60%" alt="Assigned Roles"/>

From this point, I decided to take the information that it was failing to setup the synchronization service, and went to Google. At this point I found <a href="https://www.reddit.com/r/Office365/comments/1e6n13g/azure_entra_active_directory_connect_issue_at_my/">this</a> Reddit post stating that in a some what recent update, TLS 1.2 needs to be enabled on the server for the sync to be setup. TLS is what Entra uses to securely transfer data between the on-prem server and the cloud. For anyone unaware what TLS is, it's an encryption standard, and is very common across web applications to securely transfer data. 

Luckily Microsoft made it easy for us and posted a step-by-step on how to enable TLS 1.2 <a href="https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/reference-connect-tls-enforcement#enable-tls-12">here</a>. We can either registry hack or run a PS script, I ran the script simply because it is instantaneous, but whatever suits your fancy. You can run the initial script at the top to verify whether TLS 1.2 has been successfully implemented after the fact. 

<h2>Completing the connection</h2>

Once TLS has been implemented on the server, we are done! A retry of the connection came back with a 'Configuration Complete' Message. I am now receiving Azure alerts to my email when a synchronization heartbeat is missed, and can verify the sync is setup by viewing the 'Managed Service Accounts' OU in AD. When creating Entra Connect sync a user account is automatically created that is used for the syncing process, which we can see here. 

<img src="https://imgur.com/ySliQED.png" height="50%" width="60%" alt="Assigned Roles"/>

<h2>Topology Update</h2>

Below is a picture of our environments current topology, I plan to add these at the end of each Repository to track the growth over time. Hope you enjoyed! 

<img src="https://imgur.com/zhBFu5z.png" height="50%" width="50%" alt="Current Topology"/>
