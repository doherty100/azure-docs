---
title: Moving from lab accounts to lab plans
titleSuffix: Azure Lab Services
description: 'Learn how to transition from Azure Lab Services to Azure Lab Services August 2022 Update.'
ms.topic: how-to
ms.author: rosemalcolm
author: RoseHJM
ms.date: 11/30/2022
---

# Transition from lab accounts to the improved Azure Lab Services August 2022 Update 

This article applies to users of Azure Lab Services with labs created with a lab account. If you are a brand new user to Azure Lab Services, start with [create a lab plan](tutorial-setup-lab-plan.md).

In this article, you'll learn the sequence to getting started using the features and resources made available beginning in the August 2022 update. The important update to Azure Lab Services August 2022 includes enhancements that boost performance, reliability, and scalability. It also gives you more flexibility in the way you manage labs, use capacity, and track costs. 

>[!Important]
> While you don't have to migrate to the August 2022 update of Azure Lab Services yet, we do recommend you begin using the update for all new labs.

A big part of the August 2022 update is centered around the fact that the concept *lab plans* replaces *lab accounts* in the August 2022 Update.  Although similar in functionality, there are some fundamental differences between the two concepts. The lab plan serves as a collection of configurations and settings that apply to the labs created from it. Also since the August 2022 update, a lab is an Azure resource in its own right and a sibling resource to lab plans.  You can read more about the differences between lab plans and lab accounts in [What's new in Lab Services?](./lab-services-whats-new.md#lab-plans-replace-lab-accounts).

If you're moving from the current version of Azure Lab Services to the August 2022 Update, there's likely to be a time when you're using both your existing lab accounts and using the newer lab plans. And that's ok as both are still supported, can coexist in your Azure subscription, and can even share the same external resources.

## Transition path at-a-glace

There is a bit of a mental shift to transitioning to the Azure Lab Services Update from August 2022. 

This checklist highlights the sequence at a high-level:

> [!div class="checklist"]
> - Create a lab plan
> - Request capacity for your lab plans
> - Configure shared resources
> - Create additional lab plans
> - Validate images
> - Create and publish labs
> - Update cost management reports


## 1. Create a lab plan

To begin using the update, you'll need to create a lab plan. 

If you don't already have a lab plan, you can create a temporary lab plan for requesting capacity, and delete the plan afterwards. Because capacity is assigned to your subscription, it's not affected when you create or delete lab plans. The first time you create a lab plan, a special Microsoft-managed Azure subscription is automatically created.  This subscription isn’t visible to you and is used internally to assign your [dedicated capacity](/azure/lab-services/capacity-limits#per-customer-assigned-capacity).

- [Create a  lab plan](/azure/lab-services/tutorial-setup-lab-plan).
  - This lab plan can be deleted once capacity is requested.
  - You don't need to enable advanced networking or images; or assign permissions.
  - You can select any region.

In practice, more than one lab plan might be needed depending on your scenario. For example, the math department may only require one lab plan in one resource group. The computer science department might require multiple lab plans. One lab plan can enable advanced networking and a few custom images. Another lab plan can use basic networking and not enable custom images. Multiple lab plans can be kept in the same resource group.

And, since lab accounts and lab plans cannot share capacity, you'll need to request new capacity for lab plans even if you have existing capacity for lab accounts. Before you request capacity, you must have at least one lab plan in your subscription.

## 2. Request capacity

As a customer, you're now assigned your own [dedicated VM cores quota](/azure/lab-services/capacity-limits#per-customer-assigned-capacity).  This quota is assigned per-subscription. The initial number of VM cores assigned to your subscription is limited, so you'll need to request a core limit increase.  Even if you're already using lab accounts in the current version of Azure Lab Services, you'll still need to request a core limit increase; existing cores in a lab account won't be available when you create a lab plan.

1. Verify the capacity available in your subscription by [determining the current usage and quota](./how-to-determine-your-quota-usage.md).
1. [Request a core limit increase](/azure/lab-services/how-to-request-capacity-increase?tabs=Labplans).
1. If you created a temporary lab plan, you can delete it at this point.  Deleting lab plans has no impact on your subscription or the capacity you have available. Capacity is assigned to your subscription.

#### Tips for requesting capacity

The time that it takes to assign capacity varies depending on the VM size, region, and number of cores requested. To ensure you have the resources you require when you need them, you should:

- Request capacity as far in advance as possible.
- Make incremental requests for VM cores rather than making large, bulk requests. Breaking requests for large numbers of cores into smaller requests gives extra flexibility in how those requests are fulfilled.
  - For example, when you move from lab accounts to lab plans, you should first request sufficient capacity to set up a few representative labs that serve as a proof-of-concept.  Later, you can make additional capacity requests based on your upcoming lab needs.
- If possible, be flexible on the region where you're requesting capacity.
- Capacity remains assigned for the lifetime of a subscription. You only need to request extra capacity if you need more than is already assigned to your subscription.

## 3. Configure shared resources  

You can reuse the same Azure Compute Gallery and licensing servers that you use with your lab accounts.  Optionally, you can also [configure more licensing servers](/azure/lab-services/how-to-create-a-lab-with-shared-resource) and galleries based on your needs. For VMs that require access to a licensing server, you'll create lab plans with [advanced networking](/azure/lab-services/how-to-connect-vnet-injection#connect-the-virtual-network-during-lab-plan-creation) enabled as shown in the next step.

## 4. Create additional lab plans

While you're waiting for capacity to be assigned, you can continue creating lab plans that will be used for setting up your labs.  

1. [Create and configure lab plans](/azure/lab-services/tutorial-setup-lab-plan).
    - If you plan to use a license server, don't forget to enable [advanced networking](/azure/lab-services/how-to-connect-vnet-injection#connect-the-virtual-network-during-lab-plan-creation) when creating your lab plans.
    - The lab plan’s resource group name is significant because educators will select the resource group to [create a lab](/azure/lab-services/tutorial-setup-lab#create-a-lab).
    - Likewise, the lab plan name is important.  If more than one lab plan is in the resource group, educators will see a dropdown to choose a lab plan when they create a lab.
1. [Assign permissions](/azure/lab-services/tutorial-setup-lab-plan#add-a-user-to-the-lab-creator-role) to educators that will create labs.
1. Enable [Azure Marketplace images](/azure/lab-services/specify-marketplace-images).
1. [Configure regions for labs](/azure/lab-services/create-and-configure-labs-admin).  You should enable your lab plans to use the regions that you specified in your capacity request.
1. Optionally, [attach an Azure Compute Gallery](/azure/lab-services/how-to-attach-detach-shared-image-gallery).
1. Optionally, configure [integration with Canvas](/azure/lab-services/lab-services-within-canvas-overview) including [adding the app and linking lab plans](/azure/lab-services/how-to-get-started-create-lab-within-canvas). Alternately, configure [integration with Teams](/azure/lab-services/lab-services-within-teams-overview) by [adding the app to Teams groups](/azure/lab-services/how-to-get-started-create-lab-within-teams).

If you're moving from lab accounts, the following table provides guidance on how to map your lab accounts to lab plans:

|Lab account configuration|Lab plan configuration|
|---|---|
|[Virtual network peering](/azure/lab-services/how-to-connect-peer-virtual-network#configure-at-the-time-of-lab-account-creation)|Lab plans can reuse the same virtual network as lab accounts. </br> - [Setup advanced networking](/azure/lab-services/how-to-connect-vnet-injection#connect-the-virtual-network-during-lab-plan-creation) when you create the lab plan.|
|[Role assignments](/azure/lab-services/administrator-guide-1#manage-identity) </br> - Lab account owner\contributor. </br> - Lab creator\owner\contributor.|Lab plans include new specialized roles. </br>1. [Review roles](/azure/lab-services/administrator-guide#rbac-roles). </br>2. [Assign permissions](/azure/lab-services/tutorial-setup-lab-plan#add-a-user-to-the-lab-creator-role).|
|Enabled Marketplace images. </br> - Lab accounts only support Gen1 images from the Marketplace.|Lab plans include settings to enable [Azure Marketplace images](/azure/lab-services/specify-marketplace-images). </br> - Lab plans support Gen1 and Gen2 Marketplace images, so the list of images will be different than what you would see if using lab accounts.|
|[Location](/azure/lab-services/how-to-manage-lab-accounts#create-a-lab-account) </br> - Labs are automatically created within the same geolocation as the lab account. </br> - You can't specify the exact region where a lab is created. |Lab plans enable specific control over which regions labs are created. </br> - [Configure regions for labs](/azure/lab-services/create-and-configure-labs-admin).|
|[Attached Azure Compute Gallery (Shared Image Gallery)](/azure/lab-services/how-to-attach-detach-shared-image-gallery-1)|Lab plans can be attached to the same gallery used by lab accounts. </br>1. [Attach an Azure Compute Gallery](/azure/lab-services/how-to-attach-detach-shared-image-gallery). </br>2. Ensure that you [enable images for the lab plan](/azure/lab-services/how-to-attach-detach-shared-image-gallery#enable-and-disable-images).|
|Teams integration|Configure lab plans with [Teams integration](/azure/lab-services/lab-services-within-teams-overview) by [adding the app to Teams groups](/azure/lab-services/how-to-get-started-create-lab-within-teams).|
|[Firewall settings](/azure/lab-services/how-to-configure-firewall-settings-1) </br> - Create inbound and outbound rules for the lab's public IP address and the port range 49152 - 65535.|[Firewall settings](/azure/lab-services/how-to-configure-firewall-settings) </br> - Create inbound and outbound rules for the lab's public IP address and the port ranges 4980-4989, 5000-6999, and 7000-8999.|

## 5. Validate images

Each of the VM sizes has been remapped to use a newer [Azure VM Compute SKU](/azure/lab-services/administrator-guide#vm-sizing). If you're using an [attached compute gallery](/azure/lab-services/how-to-attach-detach-shared-image-gallery), validate each of your customized images with the new VM Compute SKU by publishing a lab with the image and testing common student workloads.  Before creating labs, verify that each image in the compute gallery is replicated to the same regions enabled in your lab plans.

## 6. Create and publish labs

Once you have capacity assigned to your subscription, you can [create and publish](/azure/lab-services/tutorial-setup-lab) representative labs to validate the educator and student experience. Creating representative labs is an optional but highly recommended step, which enables you to validate performance based on common student workloads. 

### Lab strategies

You cannot migrate existing labs to the August 2022 Update. Instead, you must create new labs. Along with all the new enhancements, the requirement to create new labs provides a good opportunity to revisit your overall lab structure and plan changes where necessary.

- **Delete and recreate labs**

  Most schools delete their labs and recreate them each semester (or class session). You can schedule the move to the August 2022 Update during one of these transitions.  

- **Reuse existing labs**

  Some schools reuse the same labs each class session and change the student roster. With this approach, you must plan the creation of new labs to transition to, typically at the start of a new session.

> [!NOTE]
> Although you cannot migrate existing labs, you can still reuse other assets such as Compute Galleries and images, and any licensing servers.

## 7. Update cost management reports

Update reports to include the new cost entry type, `Microsoft.LabServices/labs`, for labs created using the August 2022 Update. [Built-in and custom tags](/azure/lab-services/cost-management-guide#understand-the-entries) allow for [grouping](/azure/cost-management-billing/costs/quick-acm-cost-analysis) in cost analysis. For more information about tracking costs, see [Cost management for Azure Lab Services](/azure/lab-services/cost-management-guide).

## Next steps

- As an admin, [create a lab plan](tutorial-setup-lab-plan.md).
- As an admin, [manage your lab plan](how-to-manage-lab-plans.md).
- As an educator, [configure and control usage of a lab](how-to-configure-student-usage.md).
