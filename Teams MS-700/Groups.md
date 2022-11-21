# important questions:

## Group and team creation, naming, classification, and guest access 
- Does your organization require a specific naming convention for teams?
- Do team creators need the ability to assign organization-specific classifications to teams?
- Do you need to restrict the ability to add guests to teams on a per-team basis?
- Does your organization require limiting who can create teams?

## Group and team expiration, retention, and archiving
- Does your organization require specifying an expiration date for teams?
- Does your organization require specific data retention policies be applied to teams?
- Does your organization expect to require the ability to archive inactive teams to preserve the content in a read-only state?

## Group and team membership management
- Does your organization require a consistent process for managing membership of one or more teams?
- Does your organization require owners, or the members themselves, to justify their continued membership of one or more teams regularly?
- Does your organization require approval for users and guests to request access to resources including teams, groups, SharePoint sites, and apps?

## Teams feature management
- Does your organization require limiting Teams features for your entire tenant?
- Does your organization require limiting Teams features for specific users?

# Creating Microsoft 365 Groups
 
## Create a new Microsoft 365 group

### Use Microsoft 365 admin center
1. Teams & groups > Active teams & groups.
2. select Add a group.
3. select Microsoft 365,
4. type a name for the group, and, optionally, a description.
5. add the name of one or more people who will be designated to manage the group.
6. add the members of the group
7. choose a privacy option and whether you want to add Microsoft Teams 
8. in the `Group email address` field, type an email address for the group.
9. From the Privacy drop-down menu, `choose Private or Public`.
10. Under Owner section, select `Select owner`, then choose the user who will be the owner of the group,

### Use PowerShell
```powershell
New-UnifiedGroup -DisplayName "Sales Department" -Alias Salesdepartment
```

### Upgrade a distribution list
Distribution lists have a long history in messaging for organizing people into groups to facilitate communication and collaboration. But distribution lists are limited to email messages, and available in SharePoint for distribution only.

#### Use Exchange admin center
1. Go to the `Exchange admin center`.
2. In the Exchange admin center, go to `Recipients > Groups`.
3. On the Groups page, `select the Distribution list tab`.You'll see a distribution lists (also called distribution groups ) that are eligible to be upgraded to Microsoft 365 groups.
4. Select a `distribution group list` that you want to upgrade to a Microsoft 365 group.
5. Select the `Upgrade distribution` group option from the top menu.
6. On the information dialog, `select Upgrade` to confirm the upgrade. The process begins immediately. Depending on the size and number of DLs, `the process can take several minutes or up to hours`.

#### Use PowerShell
```powershell 
Upgrade-DistributionGroup -DlIdentities marketing-dl@contoso.com, finance-dl@contoso.com
```
* Get the eligible DLs in the tenant and upgrade them using the upgrade command:
```powershell 
Get-EligibleDistributionGroupForMigration | Foreach-Object{
    Upgrade-DistributionGroup -DlIdentities $_.PrimarySMTPAddress
}
```
* Get the list of all DLs and upgrade only the eligible DLs
```powershell
Get-DistributionGroup| Foreach-Object{
Upgrade-DistributionGroup -DlIdentities $_.PrimarySMTPAddress
}
```

# Manage a Microsoft 365 group

## Use Microsoft 365 admin center
1. In the Microsoft 365 admin center, on the Groups page, `select the group that you want to configure`.
2. On the selected group page, `you can edit the settings`, such as managing members and owners, changing group privacy, changing email sending permissions, or creating a team from the group.

## Use PowerShell
```powershell  
Set-UnifiedGroup -DisplayName "Sales Department" -RequireSenderAuthenticationEnabled $false
```

# Strategies for Microsoft 365 Groups creation
- Open (default)  
    Users can create their own groups as needed	
- IT-led  
    Users can request a group from IT department
- Controlled  
    Group creation restricted to specific people, teams, or services	

## restrict group creations
If you run the following script in your environment, you will stop users from creating new Microsoft 365 groups unless they are a member of the security group you specified in the first line of the script.

``` powershell
$GroupName = "<SecurityGroupName>"
$AllowGroupCreation = "False"

Connect-AzureAD

$settingsObjectID = (Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ).id

if(!$settingsObjectID){

$template = Get-AzureADDirectorySettingTemplate | Where-object {$_.displayname -eq "group.unified"}

$settingsCopy = $template.CreateDirectorySetting()

New-AzureADDirectorySetting -DirectorySetting $settingsCopy

$settingsObjectID = (Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ).id

}


$settingsCopy = Get-AzureADDirectorySetting -Id $settingsObjectID
$settingsCopy["EnableGroupCreation"] = $AllowGroupCreation


if($GroupName)

{ $settingsCopy["GroupCreationAllowedGroupId"] = (Get-AzureADGroup -SearchString $GroupName).objectid

}


Set-AzureADDirectorySetting -Id $settingsObjectID -DirectorySetting $settingsCopy

(Get-AzureADDirectorySetting -Id $settingsObjectID).Values
```

# Expiration policy

> When a group expires, all of its associated services (the mailbox, Planner, SharePoint site, team, etc.) are also `deleted`.  
When a group expires, it is "soft-deleted" which means it can still be `recovered for up to 30 days`.

## Configure Microsoft 365 groups expiration policy

1. Sign-in to `Azure AD Admin Center` as a global administrator.
2. On the left pane `select Groups` section, and then `select Expiration` to open the expiration settings.
3. Next, on the Expiration page, you can specify:
- Group lifetime
- Email contact for groups with no owners
- Enable expiration for these Microsoft 365 Groups (All, Selected, None)
4. To finish the configuration, `select Save button`.

# Naming policy
















