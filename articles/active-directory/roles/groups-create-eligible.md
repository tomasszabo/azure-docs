---
title: Create a group for assigning roles in Azure Active Directory
description: Learn how to create a role-assignable group in Azure AD. Manage Azure roles in the Azure portal, PowerShell, or Graph API.
services: active-directory
author: rolyon
manager: amycolannino
ms.service: active-directory
ms.workload: identity
ms.subservice: roles
ms.topic: how-to
ms.date: 07/30/2021
ms.author: rolyon
ms.reviewer: vincesm
ms.custom: it-pro

ms.collection: M365-identity-device-management
---

# Create a role-assignable group in Azure Active Directory

You can only assign a role to a group that was created with the ‘isAssignableToRole’ property set to True, or was created in the Azure portal with **Azure AD roles can be assigned to the group** turned on. This group attribute makes the group one that can be assigned to a role in Azure Active Directory (Azure AD). This article describes how to create this special kind of group. **Note:** A group with isAssignableToRole property set to true cannot be of dynamic membership type. For more information, see [Use Azure AD groups to manage role assignments](groups-concept.md).

## Prerequisites

- Azure AD Premium P1 or P2 license
- Privileged Role Administrator or Global Administrator
- AzureAD module when using PowerShell
- Admin consent when using Graph explorer for Microsoft Graph API

For more information, see [Prerequisites to use PowerShell or Graph Explorer](prerequisites.md).

## Azure portal

1. Sign in to the [Azure portal](https://portal.azure.com).

1. Select **Azure Active Directory** > **Groups** > **All groups** > **New group**.

    [![Open Azure Active Directory and create a new group.](./media/groups-create-eligible/new-group.png "Open Azure Active Directory and create a new group.")](./media/groups-create-eligible/new-group.png#<lightbox>)

1. On the **New Group** tab, provide group type, name and description.

1. Turn on **Azure AD roles can be assigned to the group**. This switch is visible to only Privileged Role Administrators and Global Administrators because these are only two roles that can set the switch.

    [![Make the new group eligible for role assignment](./media/groups-create-eligible/eligible-switch.png "Make the new group eligible for role assignment")](./media/groups-create-eligible/eligible-switch.png#<lightbox>)

1. Select the members and owners for the group. You also have the option to assign roles to the group, but assigning a role isn't required here.

    [![Add members to the role-assignable group and assign roles.](./media/groups-create-eligible/specify-members.png "Add members to the role-assignable group and assign roles.")](./media/groups-create-eligible/specify-members.png#<lightbox>)

1. After the members and owners are specified, select **Create**.

    [![The Create button is at the bottom of the page.](./media/groups-create-eligible/create-button.png "The Create button is at the bottom of the page.")](./media/groups-create-eligible/create-button.png#<lightbox>)

The group is created with any roles you might have assigned to it.

## PowerShell

### Create a group that can be assigned to role

```powershell
$group = New-AzureADMSGroup -DisplayName "Contoso_Helpdesk_Administrators" -Description "This group is assigned to Helpdesk Administrator built-in role in Azure AD." -MailEnabled $false -SecurityEnabled $true -MailNickName "contosohelpdeskadministrators" -IsAssignableToRole $true
```

For this type of group, `isPublic` will always be false and `isSecurityEnabled` will always be true.

### Copy one group's users and service principals into a role-assignable group

```powershell
#Basic set up
Install-Module -Name AzureAD
Import-Module -Name AzureAD
Get-Module -Name AzureAD

#Connect to Azure AD. Sign in as Privileged Role Administrator or Global Administrator. Only these two roles can create a role-assignable group.
Connect-AzureAD

#Input variabled: Existing group
$idOfExistingGroup = "14044411-d170-4cb0-99db-263ca3740a0c"

#Input variables: New role-assignable group
$groupName = "Contoso_Bellevue_Admins"
$groupDescription = "This group is assigned to Helpdesk Administrator built-in role in Azure AD."
$mailNickname = "contosobellevueadmins"

#Create new security group which is a role assignable group. For creating a Microsoft 365 group, set GroupTypes="Unified" and MailEnabled=$true
$roleAssignablegroup = New-AzureADMSGroup -DisplayName $groupName -Description $groupDescription -MailEnabled $false -MailNickname $mailNickname -SecurityEnabled $true -IsAssignableToRole $true

#Get details of existing group
$existingGroup = Get-AzureADMSGroup -Id $idOfExistingGroup
$membersOfExistingGroup = Get-AzureADGroupMember -ObjectId $existingGroup.Id

#Copy users and service principals from existing group to new group
foreach($member in $membersOfExistingGroup){
if($member.ObjectType -eq 'User' -or $member.ObjectType -eq 'ServicePrincipal'){
Add-AzureADGroupMember -ObjectId $roleAssignablegroup.Id -RefObjectId $member.ObjectId
}
}
```

## Microsoft Graph API

### Create a role-assignable group in Azure AD

```http
POST https://graph.microsoft.com/v1.0/groups
{
  "description": "This group is assigned to Helpdesk Administrator built-in role of Azure AD.",
  "displayName": "Contoso_Helpdesk_Administrators",
  "groupTypes": [
    "Unified"
  ],
  "isAssignableToRole": true,
  "mailEnabled": true,
  "securityEnabled": true,
  "mailNickname": "contosohelpdeskadministrators",
  "visibility" : "Private"
}
```

For this type of group, `isPublic` will always be false and `isSecurityEnabled` will always be true.

## Next steps

- [Assign Azure AD roles to groups](groups-assign-role.md)
- [Use Azure AD groups to manage role assignments](groups-concept.md)
- [Troubleshoot Azure AD roles assigned to groups](groups-faq-troubleshooting.yml)
