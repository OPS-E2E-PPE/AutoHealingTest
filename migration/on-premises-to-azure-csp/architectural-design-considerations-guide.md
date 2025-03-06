---
title: Architectural design and considerations guide - Azure Cloud Solution Provider | Microsoft Docs
description: Review the architectural design and considerations guide for initial workload migration for Azure CSP
services: azure-csp
author: KirillKotlyarenko
manager: narena

ms.service: azure-csp
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: kirilk
---

# Architectural design and considerations guide: Initial workload migration 

This article is primarily a mentor’s guide to help technical-implementation consultants begin onboarding customers or organizations to Microsoft Azure Cloud Solution Provider (Azure CSP). The guide describes a typical process that a consultant would undergo to drive successful initial workload migration.

The article includes links to existing Microsoft technical step-by-step implementation resources and associated training materials. Such resources can help implementation consultants of varying Azure experience and expertise to navigate the process of migrating workloads to Azure. Use the article as a reference guide to Azure and the various options for migrating workloads from on-premises to Azure. 

## Create the virtual network and site-to-site VPN for the Azure virtual machines

One of the first steps an organization does in integrating an on-premises datacenter with Microsoft Azure is to stretch the on-premises datacenter network to Azure. You achieve this connectivity through a site-to-site VPN, which provides a security boundary so that the servers and workloads that are running in Azure are integrated with the stretched network of the on-premises datacenter. The network in Azure appears as part of the on-premises network, allowing servers or VMs in Azure to be joined to on-premises Active Directory and allowing servers and VMs in Azure to be patched, monitored, and managed as if they were within the on-premises datacenter.

A virtual network is required for the Azure virtual machines that are running in the cloud to cross a VPN gateway and connect back to the on-premises network. If a virtual machine in Azure is not part of a virtual network, or if the virtual network does not have a gateway, the Azure virtual machine still works but is unable to connect back to the on-premises network directly. Starting with step 1 of the Microsoft online article, [Create a virtual network with a site-to-site connection by using the Azure portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal), the guide walks you through the creation of an Azure network and a gateway as well as the creation of the VPN tunnel. 

> [!IMPORTANT]
> 
> - In step 1 of [Create a virtual network with a site-to-site connection by using the Azure portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal), you define the virtual network’s address space. Make sure that this process doesn’t overlap your on-premises address space. For performance, choose an Azure region that reasonably corresponds to your on-premises datacenter.  
> - In step 3, you specify DNS servers. You can specify the IP addresses of machines across the VPN in the on-premises network, if that is the desired configuration. You can update the addresses later if a domain controller or other DNS server is placed in an Azure virtual network after it is created.  
> - In step 5, you create the VPN gateway. This process takes some time (about one hour), and it depends on what type of device is on the other end (on-premises) of the connection. Check this list for compatible and supported VPN devices. If you don’t have any of those appliances available, you can do the same thing with remote routing and access services that are running on a Windows Server 2012 or later machine with a public IP address.   
> - In step 6, you configure the network on Azure so that the gateway knows what traffic to route. You’ll need the public IP address of the VPN endpoint that you plan to use in the on-premises network, as well as the address spaces that will be routed to on-premises through the site-to-site VPN.  
    
## Migrate the first workload

Now that you've created an Azure virtual network and established a site-to-site link with the on-premises network, you can [migrate the first workload](migrate-first-workload-to-azure-csp.md) to Azure CSP. For this example, a standalone Internet Information Services (IIS) web server will be migrated. After you've created the virtual machine in Azure, creating the web server is identical to creating an on-premises server.  

> [!IMPORTANT]
> 
> - In the "Create the Windows virtual machine" section of the [Create your first Windows virtual machine in the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-hero-tutorialn), use a size A1 virtual machine for this example.  
> - If the virtual machine is to be public facing, it needs a public IP and network security group to allow port 80/443 through on the public IP. Otherwise, you can omit the public IP. For information about network security groups and allowing access, see the "Open port 80" section in [Experiment with installing a role on your Windows VM](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-hero-role). Follow these steps after you create the virtual machine in [Create your first Windows virtual machine in the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-hero-tutorial).  

Follow the instructions in [Create a Windows virtual machine with the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-hero-tutorial) to create a Windows Server 2016 virtual machine. The article walks you through creating a virtual machine on Azure Resource Manager and networking the virtual machine.   

- There are also some valuable training videos and guides on Microsoft Virtual Academy. For a training refresher and visual step-by-step walkthrough on the creation of virtual machines in Azure, see [Deploy Virtual Machines in the Cloud: Part 1](https://mva.microsoft.com/en-US/training-courses/deploy-virtual-machines-in-the-cloud-part-1-14008?l=Auj4V0prB_5305192797) and [Deploy Virtual Machines in the Cloud: Part 2](https://mva.microsoft.com/en-US/training-courses/deploy-virtual-machines-in-the-cloud-part-2-14009?l=uGR0uqtrB_605192797).  

- If you want to create virtual machines through PowerShell, see [Create a Windows virtual machine using Resource Manager and PowerShell](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-ps-create). You can refer to it to see the PowerShell syntax for the Resource Manager operations.  

After you've created a virtual machine and it is online, it is accessible for remote access through the steps outlined in [Create a Windows virtual machine with the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-hero-tutorial). To allow connectivity for testing purposes, create a DNS entry for the virtual machine in your internal DNS server.  

You can now install Windows Server roles and applications on the newly created virtual machine, such as installing IIS through Server Manager. For step-by-step documentation, see [Experiment with installing a role on your Windows VM](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-hero-role). With IIS installed and running, you can push HTML code to the server either over the site-to-site VPN from the on-premises network using a simple *\\servername\sharename* SMB connection or by copying it through the remote desktop session.  

After you've installed the web application and code on the server, test the virtual machine to ensure functionality by doing the following:  

- Go to the webpage from *localhost* (Azure virtual machine) to make sure that all the code was successfully copied and is functioning.  

- Go to the webpage from across the site-to-site VPN from an on-premises machine by using the host name that you put into the DNS server previously to ensure that traffic is traversing the VPN correctly.  

- *If the web server is public*, go to the webpage from the public IP address. You can collect the public IP address on the **Essentials** blade of your virtual machine when you select it. For more information, see [Experiment with installing a role on your Windows VM](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-hero-role).  

After the validation has succeeded, you can fail over the DNS name for the on-premises web server (www, web, and so on) to point to the Azure virtual machine. The migration is complete, and the Azure virtual machine, rather than the on-premises server, can now serve requests.  

## Confirm basic functionality in Microsoft Azure

Now that you have a virtual network and a site-to-site VPN, and the first workload is running in Azure, you can explore other functions. 

- **Access the virtual machine**: You can access the virtual machine directly through the Azure interface by using the Windows Remote Desktop method, as documented in [Create a Windows virtual machine with the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-hero-tutorial). Or you can use Remote Desktop to access it from a machine in your on-premises network by IP or host name if you have allowed for that in your virtual machine’s firewall settings. 
- **Patch the virtual machine**: Azure virtual machines need to be patched just as on-premises systems do. These virtual machines can work with your standard patching process (for example, Windows Server Update Service, System Center Configuration Manager, or standalone patching through Windows Update). 
- **Monitor the virtual machine**: Azure virtual machines are monitored and managed just as the on-premises machines are. You can install agents (for example, [OMS](https://blogs.technet.microsoft.com/hybridcloudbp/2017/02/17/oms-in-csp/) or [Datadog](https://www.datadoghq.com/azurecsp/)) and report into the integrated monitoring solution.

## Migrate the second workload (example: A simple, single-server web and SQL configuration) 

Now that the first workload is working and you've covered some basic Azure functions, you can [migrate the second workload](migrate-additional-workloads-to-azure-csp.md). This example is similar to the first workload, except that this server is an IIS server with an integrated SQL Server back end on the same virtual machine. Where building the virtual machine is concerned, follow the instructions for the first workload. This machine must be larger than that in the first workload because of SQL Server, so this machine should be an A3 Azure instance or later. 

>[!IMPORTANT]
>- In the [Create the Windows virtual machine](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-hero-tutorial) section of the previously mentioned guide, use a size A3 virtual machine for this example. 
>- Following the previously mentioned guide [Create a Windows virtual machine with the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-hero-tutorial), create a Windows Server 2016 virtual machine on the previously created virtual network. 
>- Connect to the virtual machine, install Microsoft SQL Server, and install the IIS role as you did in the first workload. For more information, see [Experiment with installing a role on your Windows VM](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-hero-role).  
>- Back up the existing SQL database through SQL Server Management Studio. Copy the HTML code from the on-premises machine to the Azure virtual machine over the network. For more information, see [Back Up and Restore of SQL Server Databases](https://msdn.microsoft.com/en-us/library/ms187048.aspx). 
>- Restore the database in SQL Server running on Azure, and install the HTML code into IIS.  

After you've mounted the database and installed the HTML code, test the virtual machine to ensure functionality by doing the following: 

- Go to the webpage from localhost (Azure virtual machine) to make sure that all the code was successfully copied and is functioning. 
- Go to the webpage from across the site-to-site VPN from an on-premises machine by using the host name that you put into DNS server previously to ensure that traffic is traversing the VPN correctly. 
- *If web server is public*, go to the webpage from the public IP address. You can collect the public IP address on the **Essentials** blade of your virtual machine when you select it. For more information, see [Experiment with installing a role on your Windows VM](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-hero-role). 

## Migrate the second workload (example: A two-server web and SQL Server configuration) 

Another common scenario for a second workload that you might target for the migration to Azure is a two-server configuration that comprises a web server and a separate SQL server. In this example, an IIS server with a SQL back end will be migrated, but SQL Server will be on a separate virtual machine. Overall, this configuration is not much more complicated than the first and second workloads we migrated. The only addition to the process is that you need to validate network connectivity between the two machines. 

> [!IMPORTANT]
> 
> - In the [Create the Windows virtual machine](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-hero-tutorial?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) section of the previously mentioned guide, use size A2 virtual machines for this example. 
> - In the [Virtual machine creation guide](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-hero-tutorial?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), there is an option to build a virtual machine that already has SQL Server 2016 Enterprise installed. To save a step if your application already supports SQL Server 2016, choose this option instead of Windows Server 2016. However, for demonstrating Azure and for testing purposes, it is likely better to preserve parity with the on-premises application. 
> - This workload introduces a second way to migrate workloads to Azure. Both methods are viable, but the second method is more scalable to complex workloads and allows for failover without your needing to build a second set of servers. 

### Migration method 1

Following the previously mentioned guide, [Create a Windows virtual machine with the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-hero-tutorial), create two Windows Server 2016 virtual machines on the virtual network that you created previously. 

On the virtual machine that you created for SQL, install the relevant SQL Server version to the app that's being migrated. Back up the database from the on-premises SQL Server and restore it onto this Azure SQL Server virtual machine. 

On the virtual machine that you created for IIS, install IIS and copy the HTML code from the on-premises workload that's being migrated. For more information, see [Experiment with installing a role on your Windows VM](https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-windows-hero-role).  

With everything now copied to the Azure virtual machines, test the workload functionality by accessing the web server just as you did when you tested the first and second workload. 

### Migration method 2

With this migration method, you copy the virtual machines from on-premises to Azure by using Azure Site Recovery. You then fail over the virtual machines to Azure by using the test failover feature to start the virtual machines in Azure and validate their functionality. More setup is involved in this method, because you must prepare either a Hyper-V or VMware environment to copy the virtual machines to Azure. 

[Site Recovery setup and configuration guides](asr-setup-guide.md), along with guides for various virtualized environments, are part of the content framework that all program partners have access to. For more information about setting up Site Recovery in various environments, refer to those documents. Additionally, depending on the environment you are using, you can take advantage of the following articles: 

- [Hyper-V without System Center Virtual Machine Manager (VMM) (<https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-hyper-v-site-to-azure>)
- [Hyper-V with System Center VMM](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmm-to-azure)
- [VMware VMs](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmware-to-azure)

Follow the guide that suits your environment to replicate the virtual machines to Azure. The following example assumes that you are using a Hyper-V environment with VMM, so refer to that guide when reviewing the following steps:  

1. Follow the [Replicate Hyper-V virtual machines in VMM clouds to Azure using Site Recovery in the Azure portal](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmm-to-azure) guide, up to "Step 7: Test your deployment."

   > [!NOTE]
   > 
   > - The test failover does not shut down on-premises virtual machines.  
   > - If you're connecting virtual machines via the site-to-site network, we recommend that you shut down the on-premises virtual machines to avoid conflicts on the network. Do this at an acceptable time, such as during a maintenance window for testing of the application. 
   > - If you're connecting the virtual machines to an isolated virtual network, you don't need to shut down the on-premises counterparts. However, it is likely that the app that you're testing needs access to authentication and other domain controller services. Thus, you'll need some extra configuration in Site Recovery to bring a domain controller into the test mix, and the failed-over domain controller will also need some light configuration to update the DNS entries of the newly failed-over workload. 

2. Test the workload by accessing the web server and testing website functionality just as you did with the first and second workloads. 
3. At the end of the test ([5c of Step 7](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-vmm-to-azure)), the test virtual machines are cleaned up, and you can restart the on-premises virtual machines. Unlike the changes you make during a planned failover, the changes you make during your test are written back to on-premises. 
4. When the test is successful, move forward with a planned failover to bring the workload live into Azure. The on-premises counterparts replicate with the active Azure instance to stay up to date.  

## Extend on-premises Active Directory to the Azure network 

In a potential initial migration scenario where the servers are standalone systems, you might not need to add a Microsoft Active Directory domain controller in Azure if no Active Directory authentication is needed. However, as migrated workloads get more complex, you might need to join the servers to Active Directory, or you might need a domain controller in Azure. 

To add a domain controller in Azure, simply create a Windows Server virtual machine. Be sure to match the appropriate version of Windows Server on-premises Active Directory domain controllers in the organization. Then promote the server to a domain controller. The process of creating a domain controller in Azure is as follows: 

1. Follow the steps for the first or second workload earlier in this guide to build a virtual machine as a domain controller in Azure. Because this is a small test deployment, a size A2 virtual machine should be sufficient. 

2. Follow the "Install AD DS on Azure VMs" and "Reconfigure DNS Server for the virtual network" sections in [Install a replica Active Directory domain controller in an Azure virtual network](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-install-replica-active-directory-domain-controller) to promote a virtual machine to a domain controller in Azure. 

	a. We recommend that you reserve a static IP address for the domain controller. 

    b. Use the following cmdlet. Update it with your virtual machine name (in place of *AzureDC1* in this example) and your IP address (in place of *10.0.0.4* in this example) in the virtual network:  
    * Test to see that the IP is available by using: `Test-AzureStaticVNetIP -VNetName "Vnet-Name" -IPAddress 10.0.0.4`

    * Set the StaticIP and update the VM by using: `Get-AzureRMVM -ResourceGroupName “RG-Name” -Name “AzureDC1” |  Set-AzureStaticVNetIP -IPAddress 10.0.0.4 | Update-AzureRMVM`

## Summary

Now you have one or two workloads and likely an Active Directory domain controller (potentially 3-5 total virtual machines) running in Microsoft Azure. Integrate these Azure virtual machines into their normal server and application administration and management process, which includes patching, monitoring, and ongoing maintenance. 

With these initial workloads running in Azure, the next step is to quickly identify the next 12-15 servers that will be migrated to Azure, and continue migrating virtual machines to Azure via tools and automation. 

## Next steps

- Start [migrating the first workload](migrate-first-workload-to-azure-csp.md) to Azure CSP.
- Review [Architectural design and considerations guide: Advanced networking scenarios](advanced-design-considerations-guide.md).
