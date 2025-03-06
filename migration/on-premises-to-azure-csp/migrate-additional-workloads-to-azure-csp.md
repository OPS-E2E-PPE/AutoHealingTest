---
title: Migrate additional workloads from on-premises to Azure CSP | Microsoft Docs
description: Learn how to migrate additional workloads from on-premises to Azure CSP.
services: azure-csp
author: KirillKotlyarenko
manager: narena

ms.service: azure-csp
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: kirilk
---

# Migrate additional workloads from on-premises to Azure CSP

After you complete the [first migration](migrate-first-workload-to-azure-csp.md) to test the migration process and increase customer confidence, you should then perform assessment and discovery for the next wave of migrations. These migrations usually involve multi-server dependent processes.

This article provides direction on migrating those workloads, usually composed of 12-15 virtual machines.

> [!NOTE]
> This guide covers just one of many possible migration methods for Azure. The method provided here is just a suggestion. Feel free to use a different method if that works better for your purposes.

## Migration planning summary, goals, and objectives

Migration from on-premises datacenters to Microsoft Azure minimizes an organization’s need to continue to maintain and manage expensive datacenters.  An application is still the same application, regardless of whether it is running inside an organization’s datacenter, or hosted in another datacenter like Azure. 

Azure services provide an organization with additional technology solutions that benefit the business. These solutions include: 

- Datacenters geographically distributed around the globe to serve organizations anywhere in the world. 
- Geo-redundancy of the Azure datacenter infrastructure, for high availability with business continuity and disaster recovery. 
- Virtually limitless capacity to scale as organizations’ needs and requirements grow. 
- Resources needed are used on a pay “as needed” basis. Capacity can be decreased or eliminated without an organization incurring fixed capital costs for operations. 
- Availability of integrated technology services enables organizations to become agile through digital transformation. These include backup, monitoring, management, data analytics, infrastructure as a service (IaaS), platform as a service (PaaS), mobile compute services, and Internet of Things.  

Organizations of any size are no longer excluded from using enterprise-scale technology features and services. These organizations can extend their IT needs to meet core business objectives by enabling the services required by the organization. 

### Best practice: start with simpler workloads, progress to more complex

The applications and servers included in the next 15 (or greater) virtual machines being migrated to Azure may be more complex than those included in the first migration. Still, it is best not to pick the most complicated, most cumbersome, or most visible application the organization has for the next migration. Although it is great to tackle high visibility application migrations, Azure Cloud Solution Provider (Azure CSP) partners have been more successful with a series of migrations. Progressively build up to tackling the most strategic and complicated applications. 

The migration process is an iterative learning process, both for the partner and the customer. For the partner, the learning process involves understanding the change management control processes the customer has in place. Then the partner can truly internalize the existing datacenter structure and operations, and establish a good migration process between the customer’s IT team and the partner’s team. 

For the customer, the learning process involves understanding how Azure works, and how they will administer and manage their applications when they are migrated to Azure. This includes establishing all networking, security, change controls, monitoring, and ongoing maintenance practices for their day-to-day operations. 

This is primarily why partners should walk through the migration process with simple workloads to start the journey. It is during these cycles that all operational processes are updated, and IT personnel become familiarized with and trained in the hybrid cloud integration experience. From there, they can then take on more complex applications. 


### Select the subsequent workloads for migration

With this iterative migration process in mind, the next batch of 15 (or greater) virtual machines to be selected for migration would typically include: 

- **Standalone front-end and back-end applications**:  These might include Internet Information Services (IIS) web or SQL applications that have front-end and back-end server roles. These should be relatively independent applications, with limited integration with other servers and systems. This provides a good migration experience of moving servers to Azure. 
- **Load-balanced applications**:  Another good, relatively contained workload to migrate to Azure includes applications that have a load-balancing component to them, like web server farms. These workloads might involve migrating web servers and potentially implementing a virtual load balancer appliance on Azure. 
- **Replicated applications**: These include applications that might have data replication between multiple servers, such as SQL Server running "AlwaysOn Availability Groups." In such cases, existing replication occurs between servers (on-premises or between sites), and an Azure site can now host the node of the replicated environment. 

You might also migrate workloads such as contained SharePoint farms. These include load-balanced, front-end servers, and replicated back-end servers, with a combination of a handful of technical scenarios. Note that the organization may choose to migrate one or two smaller and simpler groups of servers to Azure, and then the last group of servers might be the more complicated SharePoint server farm. 

In the cloud, there is no need to overbuy capacity. An organization can buy and add more capacity at any time. Thus, "rightsizing" servers and storage to what the organization needs is significantly more cost effective than overbuying capacity, and letting it sit idle and unused. 

This is where a technical assessment of existing application workloads helps organizations determine exactly what they need. There are tools that run on existing systems, and over a two-week period collect and measure demand, performance, and capacity requirements. These tools can also provide growth projections, to help an organization select a cloud server and resource configuration that exactly meets their needs.  


### Application selection migration assessment

In the process of migrating applications from an on-premises environment to  Azure, it is common to select which applications will migrate to the cloud and which applications may not be able to. It is not a foregone conclusion that everything will be migrated. You conduct an assessment process to better understand and validate the best workloads to migrate. 
 
For example, consider an application that is no longer used or no longer useful for the organization, and is slated for retirement. Migrating this application to the cloud is a waste of time and money. Or, an organization might identify an application that needs to be upgraded. Rather than migrating an old or legacy application, and debugging it in the cloud, the organization might prefer to run an upgraded version of the application in the cloud. In other cases, an organization might decide to adopt a software as a service (SaaS) option, and therefore might not need to migrate the old application. So, the migration process is just part of a staged upgrade process in which the application might be upgraded while being migrated to the cloud.   
 
You can also assess applications based on how easy or difficult it is to migrate them to Azure. There are a variety of factors you might consider here. For example, limitations caused by application compatibility, or vendor support of an application running in the cloud. Also consider that some applications have specific hardware dependencies that cannot be replicated in a cloud environment, or other limitations like performance. When the assessment is completed and data has been gathered, partners can help customers categorize applications into the following groups: 

- **Green**: Applications that can be readily moved to Azure without any engineering or modifications. Typically, these are Azure and Azure Site Recover validated Linux and Windows workloads. (Also see the "Technical Feasibility Guide" in section 4 of the program support portal.) These may include stateless applications and User Acceptance Test/Dev/Test labs. 
- **Yellow**: Applications that require some level of reengineering or refactoring. They can be moved to Azure in a hybrid fashion, or physical roles can be moved to Azure PaaS solutions (as this requires an architecture overhaul).  
- **Red**: Applications that cannot be moved to Azure due to legal, security, regulatory, business, or high cost of re-engineering reasons. These applications should be revisited later in the migration process.   

### Technical assessment tools

There are several tools available that gather runtime statistics of the application, and provide recommendations regarding a migration of virtual machines to Azure CSP:
- [Cloudamize](http://www.cloudamize.com/azurecsp)
- [RISC Networks](http://cloudscape.riscnetworks.com/microsoft-azurecsp/)
- [ATAData](http://www.atadata.com/azurecsp)

The data gathered from such tools is usually consolidated into reports for assessment. The reports often include: 

- Inventory. Identification of all the nodes (physical and virtual machines), and applications running on these nodes. 
- Performance analysis. Collection of performance metrics, including recommendations for Azure virtual machine instance sizes and storage options. 
- Infrastructure performance. Key metrics summarized by the tools include: 
	- Peak CPU utilization  
	- Allocated and peak RAM usage  
	- Observed storage on-premises (capacity and current occupancy) 
	- Disk IOPS and bandwidth  
	- Throughput  
	- Usage patterns (identifies how often compute and storage resources are on, idle, and unused) 
- Azure configuration and cost projections. Projections on the recommended Azure virtual machine sizes (including CPU, memory, and disk storage) that correspond to on-premises servers. The projections also include anticipated monthly costs, according to the recommended workload configurations. Use this data to run projections and cost comparisons between existing operational cost outlays and the monthly cost to run the same workload in Azure. 

### Common assessment pitfalls

It is important to run an accurate cloud workload assessment and cost analysis. Organizations make mistakes that result in inaccurate total cost of ownership (TCO) and cost comparison analysis. Common pitfalls are: 

- **Sizing and pricing overcapacity configurations**:  Frequently, organizations simply take existing system configurations (for example, 16-core processors, 256 GB RAM, 10 TB storage) and price for the exact same configuration in Azure. But the on-premises server is only running at 5 percent or 10 percent utilization. This will result in pricing 10-20 times more expensive than needed to run the workload. Thus, the tools are helpful in accurately advising an organization that they only need, say, a 2-core machine with 26 GB RAM. 
- **Storage misalignment**: Disk IOPS and bandwidth are too often overlooked when an organization is selecting a cloud server configuration. Every type of disk has limitations for these measures, and underestimating your requirements can have a negative impact on performance. Under-sizing storage performance metrics can cause sluggishness of an application. The tools go beyond what you think you might need for storage performance requirements, and provide you a report of what it recommends the application needs in an Azure configuration. 
- **Assessment over time**: The tools typically run for a period of two weeks, assessing peak loads and troughs over that time period. Simply using average performance numbers doesn't account for peak times, or doing an assessment at night that might not accurately reflect normal runtime conditions. Instead, the tools provide high and low ranges, as well as idle and busy time statistics. 
 
## Migrate subsequent servers to Azure

With the assessment complete, your next step is to begin the actual migration process. 

### Extend advanced networking integration components

Some organizations now may need to improve the simple site-to-site VPN network connection implemented for the first couple of workloads migrated to Azure. They may need to expand the networking to include Border Gateway Protocol routing, create connections between virtual networks, or configure additional gateways into Azure. For more guidance on advanced network connectivity, see the [Architectural design and considerations guide: advanced networking scenarios](advanced-design-considerations-guide.md) article. 

### Use Azure Site Recovery for workload migration

Azure Site Recovery is a common tool to move complex workloads from an on-premises datacenter to Azure. Rather than rebuilding servers and installation applications from scratch or manually exporting and importing virtual machines, Azure Site Recovery replicates entire machine states from on-premises to Azure. 

Azure Site Recovery encapsulates the underlying operating system, the applications, and the data as part of the replication process. The tool is built to support the migration of physical servers, virtual machines running on Microsoft HyperV, and VMware. 

You can also configure Azure Site Recovery to bring together server dependencies. If a workload requires 8 servers to be migrated all at the same time, the tool can encapsulate, replicate, and migrate all 8 servers together. 

### Azure Site Recovery capacity planning, setup, and configuration

For details regarding capacity planning, setup, and configuration, see [Azure Site Recovery capacity planning](asr-capacity-planning.md) and [Azure Site Recovery setup guide](asr-setup-guide.md).

These documents are step-by-step guides to set up and configure Azure Site Recovery services for migrating Hyper-V, VMware, and physical-based VMs and workloads to Azure. Specifically, these documents cover how to set up and configure the Configuration Server and Mobility Services components for on-premises environments. They also cover how to set up the Azure Site Recovery components in Azure to perform migrations. 

### Migration of specific workloads

Common workload migration scenarios are covered in the following documents:

- [Migrate SQL Server 2012 from on-premises to Azure CSP](https://azurecsp.blob.core.windows.net/files/migrate-sqlserver-2012-to-azure-csp.pdf)
- [Migrate Dynamics AX 2012 R3 from on-premises to Azure CSP](https://azurecsp.blob.core.windows.net/files/migrate-dynamics-ax-2012r3-to-azure-csp.pdf)
- [Migrate SharePoint Server 2013 from on-premises to Azure CSP](https://azurecsp.blob.core.windows.net/files/migrate-sharepoint-2013-to-azure-csp.pdf)
- [Migrate SharePoint Server 2010 from on-premises to Azure CSP](https://azurecsp.blob.core.windows.net/files/migrate-sharepoint-2010-to-azure-csp.pdf)
- [Migrate BizTalk Server 2013 R2 from on-premises to Azure CSP](https://azurecsp.blob.core.windows.net/files/migrate-biztalk-2013r2-to-azure-csp.pdf)
- [Migrate RedHat Enterprise Linux with WordPress site from on-premises to Azure CSP](https://azurecsp.blob.core.windows.net/files/migrate-rhel-and-wordpress-to-azure-csp.pdf)
- [Migrate Oracle Enterprise Linux with Oracle Database from on-premises to Azure CSP](https://azurecsp.blob.core.windows.net/files/migrate-oracle-linux-and-database-to-azure-csp.pdf)

These documents are detailed guides describing the steps for migrating common on-premises workloads, running in different environments, to Azure CSP by using Azure Site Recovery.

## Next steps

- Review [Azure Site Recovery capacity planning guide](asr-capacity-planning.md).
- Review [Azure Site Recovery setup and configuration guide](asr-setup-guide.md).
