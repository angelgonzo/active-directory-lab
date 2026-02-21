## Installing Domain Controller

With all the Network Configurations out and done, I'm able to finally to install Domain Controller.

## Roles and Features Wizard
I'm going to use the Server Manager to do this installation.

Using the Roles and Features Wizard it allows me to change my basic server into one that is more customized towards my needs, in this case it'll be a Domain Controller
[ENTER SCREENSHOT]

1. I selected 'Role-based or feature-based installation' for the installation type, as the installation will be on this local, physical server
[ENTER ROLE BASED SCR]
2. I will be using the 'winserver2025' server that I created
    - I know that there are two IPs present: 192.168.0.10 and 192.168.0.240
    - I had to install virtio tools, and one of the steps for my config was to create a new network bridge so the drivers could be downloaded
    - The main IP is 192.168.0.240
[DEST SERVER SCR]
3. In the Server Roles tab we are able to choose what roles and services we want to install on our server, in my case I went with 'Active Directory Domain Services
    - Due to the fact that I am relative new to IT, a lot of these other service don't make much sense to me but I hope with time those gaps will be filled in
    - The few exceptions are:
        - Remote Access
        - Remote Desktop Services
        - Windows Server Update Services
        - Hyper-V
        - DCHP Server
        - DNS Server
        - Device Health Attestation
[ENTER SCREEN SHOT]
4. For Select features page, I chose to leave that as default since I'm not too familiar with it.
[SCRSHOT]
5. Active Directory Domain Service simply let's me know about what the role will do and what data it will capture and store, as well as recommendations. Nothing to change here.
[SCRSHOT]
6. Lastly, we install the role

Things to keep in mind, once I installed this role, the domain controller is unconfigured. I just installed the tools necessary to be able to configure it.
It's like buying a piece of furniture, you now have all the tools, and the pieces but it's still not built.

The server is now a Domain Controller.
[ENTER SCREENSHOT]
---
## Notes
Most orgs will typically have at least 2 Domain Controllers, to ensure that if one fails, the other DC can still allow auth for the users.
- Typically these DC are duplicates, basically in a RAID 1 configuration.
- This way there's always a copy intact, and be used to restore in case new updates disrupt services
- Orgs also have DC in remote locations to improve reliablity

But for me, this won't be necessary as it's just a lab. I will be making a new domain as I don't have a pre-existing one.
---
## Domain Controller Configuration
Inside the Active Directory Domain Services Configuration Wizard, I was prompted with 3 options:
- Add a domain controller to an existing domain
- Add a new domain to an existing forest
- Add a new forest

A forest is refering to an umbrella.
Under this umbrella, there could be multiple domain controllers that all connect to that one forest.

For example, say LG acquires Panasonic, both companies have their own domains, but once LG acquires Panasonic then the question of which domain to use begins to rise. One way to solve this is to keep both domains but have Panasonic's domain connect back to the forest built by Nikon.

For me, I will create a new domain or a new forest.
winserver.local

- I chose .local because this is what is typically used for testing, if I tried to search this domain on the internet nothing will pop up
[SCREENSHOT]

### Domain Controller Options
I'm given the option to select a functional level, now from my understanding this is refering to which version of Windows Server will the Domain Controller run on. This is incase that an org has multiple DC on different version you would select the functional level of the lowest version of Windows Server a DC is running to ensure it doesn't break

Since I only have 1, my functional level for both will be Windows Server 2025

Since this is a homelab, the password I chose will be the same one for my admin account, but in the real world this should be completely different and more secure.
[SCREENSHOT]

### Additonal Options 
The NetBIOS domain name: WINSERVER

### Paths
I will leave the paths as default
[SCREEN SHOT]

There are reasons why you would want to change the paths:
- Hard drive is not big enough
- Want to put it on faster storage
- etc.

### Install
Once we finish installing, configuring the role, and rebooting the host machine, it seems like there's been a change

The admin account is now called WINSERVER\Administrator
- This means that this account is connected to my domain
[SCREENSHOT]
---
### Post Install Checks
With new configurations, it's always good to check if our settings changed. In this case, I'm interested to see if our dns settings changed

- cmd: ipconfig /all to see those settings
[SCREENSHOT]

Unfortunately, we did lose our secondary dns server and only retained the local host
I'll add it back for the ability to query to the internet

Secondary DNS: 8.8.8.8
[SCREENSHOT]
---
This is the end of the domain controller installation and configuration.

Honestly, it was pretty simple so far, however, there are a lot of things that interest me about those roles I saw during the Role Selection instance.
I also think it's fascinating that domains can be migrated to a forest from another without too much hassle? Maybe lol, I think the concept is much more interesting that the practice but regardless I hope I get a chance to take a crack at that.

So far, I've been enjoying this process of slowly building this up, piece by piece.
As the domain controller portion is wrapping up, I'll need a user computer to connect to the domain.
