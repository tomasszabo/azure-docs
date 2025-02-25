---
title: Azure Lab Services within Microsoft Teams
description: Provides an overview of using Azure Lab Services within Microsoft Teams. 
ms.topic: conceptual
ms.date: 10/07/2020
---

# Azure Lab Services within Microsoft Teams

Azure Lab Services can be leveraged within Microsoft Teams using **Azure Lab Services** Teams App. Any team owner with owner/contributor/creator access to the lab plan will be able to create labs and provision virtual machines to everyone on the team.

This article outlines the benefits of using Azure Lab Services within Teams and provides links to other articles for instructions on how to create and manage labs within Teams.

> [!NOTE]
> **Azure Lab Services** Teams App can be added only to a team, it cannot be added to individual chats or group chats.

## Benefits

Azure Lab Services integration with Microsoft Teams will help educators set up a classroom environment and provide virtual lab environments within the team(class):

* Educators can set up labs so the students can access their VMs from within Teams, without leaving Teams and having to navigate to the Lab Services web portal: [https://labs.azure.com](https://labs.azure.com).
* Single Sign-on (SSO) from Teams to Azure Lab Services.
* Team and Lab owners need not maintain class rosters in two different systems - Lab user list is autopopulated from the Team membership and a sync is performed every 24 hours automatically.
* After the initial publish of the Template VM, Lab Capacity (that is, number of VMs in the lab) would be automatically adjusted based on the addition/deletion of users from the Team membership.
* Team and Lab Owners will view only the labs related to the team and students will view only the VMs, which are provisioned for the specific team.
* Users will be auto registered to the lab and VMs will be automatically assigned upon the first login after the lab is published. Educators don't need to send invitations and students don't need to register for the lab separately.  

## Next steps

See the following articles:

* [Configure Microsoft Teams to access lab plans](how-to-configure-teams-for-lab-plans.md).
* [Create and manage labs within Teams](./how-to-manage-labs-within-teams.md).
* [Access a VM within Teams – Student view](how-to-access-vm-for-students-within-teams.md).
