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

```powershell
Get-ADUser -Filter * -Properties samAccountName | select samAccountName
```

<img src="https://imgur.com/zhBFu5z.png" height="50%" width="80%" alt="Current Topology"/>
