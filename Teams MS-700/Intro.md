# chat 
* Private chat
    - One-on-one chats: chats with one other person.
    - Group chats: chats with a few people at once, but outside of a channel.

* Channel messages
    - Standard channels: Standard channels are visible to all team members
    - Private channels: Private channels create focused spaces for collaboration within a team. 
    - Shared channels: Shared channels create collaboration spaces where you can invite people who are not in the team.

# team 
- Org-wide teams provide an automatic way for everyone 
- Public teams are open and anyone within the organization can join.
- Private teams consist only of invited users.

# Microsoft 365 Groups privacy
* Public group  
    Any user in your organization can join public groups without the need of an administrator
* Private group 
    Content in a private group can only be seen by the members of the group.

# Group membership and roles
* Owners
* Members
* Guests

# Compare groups
* Microsoft 365 Groups  
    Used for collaboration between users, both inside and outside your company
* Distribution groups  
    Used for sending notifications to a group of people	
* Security groups  
    Used for granting access to resources and for managing devices.	
* Mail-enabled security groups  
    Same as security groups but includes email distribution to members.

# Admin roles
* Teams Administrator
* Teams Device Administrator
* Teams Communications Administrator
* Teams Communications Support Engineer
* Teams Communications Support Specialist

## Assign Teams admin roles in Microsoft 365 admin center
1.  Sign-in to the Microsoft 365 admin center 
2. Select Users, and then select Active Users.
3. Select the user 4. Select Manage Roles.
5. Select Admin center access.
6. Select the role to assign to the user

## PowerShell
```powershell
$userName="LynneR@contoso.com"
$roleName="Teams Service Administrator"
$role = Get-AzureADDirectoryRole | Where {$_.displayName -eq $roleName}

Add-AzureADDirectoryRoleMember -ObjectId $role.ObjectId -RefObjectId (Get-AzureADUser | Where {$_.UserPrincipalName -eq $userName}).ObjectID
```

# Teams management tools
* Teams admin center
* Teams PowerShell module
* Teams Graph API
* Teams clients
* Azure Active Directory admin center
* Microsoft 365 admin center
* Defender portal and the Microsoft Purview compliance portal

# Plan for Teams rollout
* Get started 
    - The starting point for any Teams deployment begins with familiarizing stakeholders with their new collaboration and communication client.

* Chat, teams, channels, & apps 
    - To help drive user adoption of Teams, you should look for a quick win in the deployment process. Most organizations already use chat applications and services. The quickest win typically involves integrating these persistent chat capabilities and apps into Teams. The integration lets users continue using the legacy solutions they’re comfortable with, avoiding disruption to their daily activities.

* Meetings & Conferencing 
    - Implementing meetings and conferencing in a Teams deployment is typically done later in the rollout process. The success of adopting meetings and conferencing in Teams makes it easier to deploy the next capability voice.

* Voice 
    - The last step in a rollout is the full voice integration of PSTN calling into Teams. 

## Get familiar with Teams
- Step 1: Create the first teams and channels
- Step 2: Onboard early adopters
     - Send a welcome email to users with the names and descriptions of the teams you created and invite them to join the conversations in Teams.
- Step 3: Monitor usage and feedback
    - Use usage reports to better understand usage patterns, and along with user feedback. 
- Step 4: Get resources to plan an organization-wide rollout
    - it's time to begin your rollout of Teams to the rest of your organization.

### Microsoft FastTrack
With the FastTrack program, Microsoft provides guidance for planning, deployment, and adoption, including remote access to Microsoft engineering expertise, best practices, tools

# Plan for Teams licensing

## Teams (free) license
Free version of Microsoft Teams is best for businesses that need to get started quickly with secure online meetings, chat, and cloud storage—all in one app, for free.

## Microsoft Teams add-on licenses
Besides basic Teams features, there are add-on licenses for more features such as audio conferencing, Teams Phone, calling plans, and Microsoft Teams rooms.

* Teams Phone with Calling Plan bundle
* Teams Phone Standard
* Microsoft Teams Calling Plans
* Communication Credits	
* Audio Conferencing
* Microsoft Teams Rooms
    - Microsoft Teams Rooms Basic
    - Microsoft Teams Rooms Pro

# Deploy Microsoft Teams clients

## Windows Operating Systems
- As a part of the Microsoft 365 Apps for enterprise installer.
- MSI files 
- A standalone (.exe) 
### The Windows client uses the following locations:
- %LocalAppData%\Microsoft\Teams
- %LocalAppData%\Microsoft\TeamsMeetingAddin
- %AppData%\Microsoft\Teams
- %LocalAppData%\SquirrelTemp

## Mac Operating Systems
1. From the Teams download page, under Mac, select Download.
2. Double select the PKG file.
3. Follow the installation wizard to complete the installation.
4. Teams will be installed to /Applications folder; the installation is machine-wide.

## Linux Operating Systems
* DEB https://packages.microsoft.com/repos/ms-teams stable main
* RPM https://packages.microsoft.com/yumrepos/ms-teams

## Teams for Virtualized Desktop Infrastructure
1. Download the Teams MSI package 
2. Install the MSI to the VDI VM by running
```console
msiexec /i <path_to_msi> /l*v <install_logfile_name> ALLUSERS=1
```

# Plan for lifecycle management
##  Team user types and admin roles 
- Team creator  
    has permissions to create a group or team in the directory.
- Team owner  
    manages membership and settings for the team.
* Team member  
    is a member of your organization who participates in a team.
* Guest  
    a user who's external to your organization.

## Teams lifecycle stages

### Stage 1: Beginning
* What is the team’s purpose?
* Who belongs on the team?
* Will the team be private or public?
Can new members add themselves or do team owners add them?
* Who will have permissions to create channels or add tabs, bots, and connectors?
* What initial channels will be added to the team?
### Stage 2: Middle
* Who will monitor usage to identify problems?
* What metrics will be used to determine whether a team is healthy?
* Identify any teams that have reached the end of their useful life.
* Identify unhealthy teams that still serve a purpose but need revitalizing.
### Stage 3: End
When a group or team is deleted, most of the information in the connected services is also deleted with some exceptions. 
> Soft delete and can be reversed for up to 21 days after deletion.
- Guests   
    guests `aren't removed`
- SharePoint  
     will `not delete` the folder
- Planner  
    will result in the `deletion` of any associated plans
- Forms  
    the `deletion` of any associated forms
- Power Automate  
    Flows created in Power Automate and associated with a group or team `don't belong to the group` 
- Power BI  
    it can be copied from the existing workspace to another workspace
- Dataverse for Teams  
    can be `deleted` from within the team by the team owner. 
- Project  
assign the project to another group

# Plan for Teams governance

* who can create 
    - sharepoint
    - team
    - shared lib (oneDrive)
    - Group 

* Naming Conventions
    - prefix 
    - suffix 
    - Custom Blocked 

* Configure Guest Access 
    - who can add guest users 
    - sharing options for guest 
    - turn on or off guest access
    - configure external sharing 

* Configure expiry 
    - Set expiration duration 
    - choosing groups policy to apply 

* Seat policies 
    - Retention policy
    - eDiscovery policy
    - Data loss prevention 

* use of templates 
    - team templates 
    - sharepoint site 
    - themes 


