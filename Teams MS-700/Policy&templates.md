# Plan policy assignment for users and groups
## Types of policies
- Policy packages  
    A policy package is a collection of predefined policies and settings you can assign to users who have similar roles in your organization.
- Meeting policies  
    A meeting policy is used to control the features that are available to meeting participants for meetings scheduled by users in your organization.
- Voice and calling policies
    Voice and calling policies manage these settings through teams such as emergency calling, call routing, and caller ID.
- App policies  
    App policies are used to control applications in Microsoft Teams, such as allowing or blocking which apps users can install.
- Chat, teams, and channel policies	
    Chat, teams, and channel policies control teams and collaboration feature availability.
- Teams feature policy  
    Update policies are used to manage Teams and Office preview users that will see pre-release or preview features in the Teams app.

## Ways to assign policies
* Assign policies directly to users
    - Assign a policy directly to an individual user.
    - Assign a policy to a batch of users.
    - Assign a policy to a group.
* policy packages 
    - Assign a policy package directly to an individual user.
    - Assign a policy package to a batch of users.
    - Assign a policy package to a group.

## Policy precedence
- user 
- group 
- org wide 

## Policy packages
### View policy packages
1. In the left navigation of the `Microsoft Teams admin center`, select `Policy packages`, and then select a policy package by selecting to the left of the package name.
2. `Select the policy` you want to view.

### Customize policy packages
1. In the left navigation of the `Microsoft Teams admin` center, do one of the following:
    - Select `Policy package`s, and then select the policy package by clicking to the left of the package name.
    - Select the `policy type`. For example, select Messaging policies.
2. `Select the policy you want to edit`. Policies that are linked to a policy package have the same name as the policy package.
3. `Make the changes` that you want, and then select Save.

### Assign policy packages
1. In the left navigation of the `Microsoft Teams admin` center, go to `Users > Manage` users, and then select the user.
2. On the user's page, `select Policies`, and then next to Policy package, select Edit.
3. In the `Assign policy package pane, select the package you want to assign`, and then select Save.

####  Assign policy packages using PowerShell 
```powershell
Grant-CsUserPolicyPackage -Identity 1bc0b35f-095a-4a37-a24c-c4b6049816ab,johndoe@example.com -PackageName Education_PrimaryStudent
```

### Assign a policy package to multiple users
1. In the left navigation of the `Microsoft Teams admin` center, go to Policy packages, and then select the policy package you want to assign by selecting to the left of the package name.
2. Select `Manage` users.
3. In the `Manage users` pane, search for the user by display name or by user name, select the name, and then `select Add`. Repeat this step for each user that you want to add.
4. When you're finished adding users, `select Save`.
#### using powershell
```powershell
New-CsBatchPolicyPackageAssignmentOperation -Identity 1bc0b35f-095a-4a37-a24c-c4b6049816ab,user1@econtoso.com,user2@contoso.com -PackageName Education_PrimaryStudent
```

### Assign a policy package to a group
1. Sign in to the `Teams admin center`.
2. In the left navigation, go to the `policy package page`.
3. Select the `Group policy assignment` tab.
4. Select `Add group`, and then in the Assign a policy package to group pane, do the following:
    a. Search for and add the group you want to assign the policy package to.
    b. Select a policy package.
    c. Set the ranking for each policy type.
    d. Select Apply.
5. To manage ranking for a specific policy type, navigate to the specific `policy page`.
6. To reassign a policy package to a group, first remove the group policy assignment. Then, follow steps 1-5 to assign the policy package to a group.

#### using powershell
```powershell
Grant-CsGroupPolicyPackageAssignment -GroupId "dae90bb4-120f-4a3e-a15d-30f142e79f69" -PackageName "Education_Teacher" -PolicyRankings "TeamsAppSetupPolicy, 1", "TeamsMeetingBroadcastPolicy, 1", "TeamsMeetingPolicy, 2"
```

### Teams preview features

#### Create a custom Update policy
1. Sign in to the Teams admin center.
2. Select Teams > Teams update policies.
3. Select Add.
4. Name the update policy, add a description, and select one of the following options:

### Assign the custom Update policy to users
1. Go to `Teams admin center > Teams > Teams update policies`.
2. Select the custom Update policy.
3. Select Assign users.
4. Search and select Add next to the users.
5. Select Apply.

# templates
## types of team templates
- Base template
- Custom template

## Create a custom template
1. Log in to the Teams admin center.
2. In the left navigation, expand Teams > Team templates.
3. Select + Add.
4. In the Team templates section, select a way to create the template.
    - Create a brand new template
    - Use an existing team as a template
    - Start with an existing team template

## templates policies
1. Sign in to the `Teams admin center`.
2. Expand `Teams > Templates policie`s.
3. Select Add.
4. In the Templates Policies Settings section, complete the following fields:
    - Templates Policy name
    - Templates Policy short description
5. In the Viewable Templates table, select the `templates you want to hide` and select Hide.
6. To unhide certain templates, scroll to the Hidden templates table.
7. Select the templates to unhide, and then select Show.
8. The selected templates will appear in your Viewable templates table.
9. Select Save.

