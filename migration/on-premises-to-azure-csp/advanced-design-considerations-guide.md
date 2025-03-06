---
title: Architectural design and considerations guide for advanced networking scenarios for Azure CSP migrations | Microsoft Docs
description: Review the architectural design and considerations guide for advanced networking scenarios for Azure CSP migrations
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

# Architectural design and considerations guide: Advanced networking scenarios 

At this point in the migration process, an organization has migrated the initial three to five simple workloads (virtual machines) to Azure. The organization is also familiar with Azure administration and management, and has run discovery and assessment tools to assess the next series of workloads that are going to be migrated to Azure. 

With an additional 12-15 servers being targeted for migration to Azure, the organization might need to extend its initial network integration to Azure with a more advanced networking solution. In addition, more robust networking scenarios might be necessary to provide redundant connections to Azure to implement high availability, load balancing, integrated virtual networks, or Border Gateway Protocol (BGP) configurations. 

## Advanced networking solutions

With plans to migrate additional workloads to Azure, organizations typically want to improve redundancy and high availability. They want to exceed the redundancy and availability of the initial single virtual network that was established in the migration of the first few workloads to Azure. 

Within Azure, you can connect virtual networks that span multiple Azure regions or have multiple connections from on-premises datacenters to an Azure virtual network. You can also advertise your Azure subnets to the on-premises network with your existing BGP. All these elements work together to increase the redundancy and availability of your Azure workloads. 

### Connect a virtual network to another virtual network 

In the initial Azure site-to-site network connection, the network connection from the customer’s primary datacenter to Azure was created, establishing an extension of the customer’s infrastructure into Azure. Now, based on how geo-distributed your customer’s infrastructure is, you need to account for either recreating or consolidating resources into corresponding Azure regions and virtual networks.

Following is an example of a geo-distributed on-premises infrastructure and the corresponding Azure virtual networks that must be created from a 1:1 standpoint (not from the standpoint of consolidating sites):

|On-premises site|On-premises region|Azure virtual network|Azure region|
|---|---|---|---|
|DC1|Seattle, WA|SEA-VNET|West-US (WUS)|
|DC2|London, UK|LON-VNET|West-Europe (WEU)|
|DC3|Mumbai, India|MUM-VNET|West-India (WEI)|

The next step is to create the remaining Azure virtual networks for DC2 and DC3, after which VNet-to-VNet peering must be established between the sites for internetwork communications as follows:

- SEA-VNET to LON-VNET
- SEA-VNET to MUM-VNET 
- LON-VNET to MUM-VNET

If you want to have disaster recovery sites within Azure, then you need to ensure that the disaster recovery sites are configured the same way as your standard Azure sites with VNet-to-VNet peering established between them for communications. 

>[!IMPORTANT] 
> - If a virtual network and gateway has already been created or already exists, start at [Step 6](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal) of the [Configure a VNet-to-VNet connection using the Azure portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal) guide to create the secondary virtual network. 
> - A VNet-to-VNet connection does not have to span multiple Azure regions. However, if it does, it provides geo-redundancy. 
> - If only the first virtual network has a site-to-site VPN connection, the on-premises network can still access the second virtual network by way of the first virtual network's VPN gateway, though this is a single point of failure. 
> - Each virtual network can have only one virtual network gateway. Each gateway can have a maximum of 10 VPN tunnels, with the exception of high-performance gateways, which can have up to 30 tunnels. A VNet-to-VNet connection consumes a tunnel just as a site-to-site connection does. Keep this in mind when designing your network topology. 

Following the [Configure a VNet-to-VNet VPN gateway connection using the Azure portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal) guide, provision a second virtual network and virtual network gateway and connect it to the first virtual network. This guide assumes there is not an existing virtual network. This means that if you're using an existing network, such as the one from the start of this document, pick up at [Step 6](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal). 

After the networks are connected, you can migrate workloads to the second virtual network, which now offers high availability to the workloads in the first network. Workloads such as SQL Server AlwaysOn Availability groups can benefit from such a setup. 


### Configure BGP on an Azure VPN Gateway

Another common request that organizations make when they want to extend Azure is to tie the networking in Azure to the customer’s existing BGP network configuration. BGP is a common routing protocol that advertises network segments and routes as part of a networking environment. With BGP configured and enabled, Azure routes and networks interact better with on-premises BGP advertisements and routing management. 

Configuring BGP on your Azure VPN gateway enables Azure networks to advertise the routes into the on-premises network without having to add static routes. This is also a stepping stone to the next example: multiple and redundant gateways to Azure. 

> [!IMPORTANT]
> 
> - The following guide uses Azure Resource Manager and Azure PowerShell. Instructions for adding the AzureRM module can be found at [Overview of Azure PowerShell](https://docs.microsoft.com/en-us/powershell/resourcemanager/).
> - The [How to configure BGP on Azure VPN Gateways using Azure Resource Manager and PowerShell](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps) guide specifies adding the Autonomous System Numbers (ASN) when the gateway is created. If you're working with an existing network, the ASN can be added to an existing gateway through [PowerShell](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.network/v3.0.0/set-azurermvirtualnetworkgateway). This cmdlet replaces  item #2 of [Step 2 - Create the VPN Gateway for TestVNet1 with BGP parameters](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps). See example 1 of the linked cmdlet for the exact syntax. 
> - Azure reserves certain ASNs that cannot be used by tenants in VPN gateways. Consult the [Azure BGP overview](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-bgp-overview) to make sure the desired ASN is not reserved. The overview also contains a FAQ regarding the Azure BGP implementation. 
> - In the "Getting started with BGP on Azure VPN gateways" section of the [How to configure BGP on Azure VPN Gateways using PowerShell guide](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps#getting-started-with-bgp-on-azure-vpn-gateways), follow the steps for configuring the virtual network for BGP. The virtual network that was created at the beginning of this document or the virtual network that you created earlier with the VNet-to-VNet connection can be reconfigured for BGP routing, and it does not require creating all new networks. 
> - After you've finished this configuration, check the advertised BGP routes in the on-premises router to be sure peering is successful and that the correct routes are advertised. 


### Configure a secondary gateway to Azure

Just as multiple virtual networks enable redundancy and even geo-redundancy on the Azure side of networking, configuring a secondary gateway in Azure allows for the interconnection to multiple on-premises networks. Multiple gateways increase the redundancy of the VPN connection as well as connecting the on-premises network from multiple locations. 

>[!IMPORTANT] 
> - Configuring Azure virtual network gateways to be in an active-active configuration requires the high-performance SKU. 
> - The [Configure active-active S2S VPN connections with Azure VPN Gateways using Azure Resource Manager and PowerShell](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-activeactive-rm-powershell) guide specifies adding the ASN when the gateway is created. If you're working with an existing network, the ASN can be added to an existing gateway through [PowerShell](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.network/v3.0.0/set-azurermvirtualnetworkgateway). This cmdlet replaces item #2 of [Step 2 - Create the VPN gateway for TestVNet1 with active-active mode](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-activeactive-rm-powershell). See example 1 of the linked cmdlet for the exact syntax. 
> - There are multiple ways to configure a secondary gateway to Azure. The overview in [Highly available cross-premises and VNet-to-VNet connectivity](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-highlyavailable) covers the various methods and requirements. The example here uses the guide to set up an active-active configuration by using BGP and dual on-premises gateways for the highest reliability and redundancy. 
> - Following the [Configure active-active S2S VPN connections with Azure VPN Gateways guide](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-activeactive-rm-powershell), configure the virtual network gateway for high availability. As in the last section, the guide assumes that you are creating new networks. This is not necessary if they already exist. [Part 4](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-activeactive-rm-powershell) covers reconfiguring an existing gateway to be an active-active gateway by assigning the second IP and updating the SKU. From there, you can continue with [Part 1](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-activeactive-rm-powershell), Step 2, because the ASN  still needs to be set along with the rest of the BGP configuration. 
> - After both gateways are configured on the Azure side and the on-premises side, this configuration will be both highly available and redundant. Traffic will be balanced between the four tunnels, though traffic state will be respected. There might be a performance increase, but the goal is availability more than performance. 

### Configure an Azure load balancer 

In this scenario, it's likely that workloads need to be highly available. We recommend an Azure load balancer for distributing traffic among healthy instances. It can be configured to balance Internet traffic or internal traffic to the load-balanced set. In addition, specific monitors can be created for the load balancer to check the health of the members. 

Following the [Create an internal load balancer using PowerShell](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-get-started-ilb-arm-ps) guide, deploy an internal load balancer and add network interfaces to workloads that are in need of high availability. Test connectivity from the on-premises network to the load balancer IP to ensure that local network machines can access the workload behind the Azure load balancer. Depending on how the workload handles load-balanced connections, you might need to adjust the distribution mode (“stickiness” or affinity) of the load balancer as detailed in [Configure the distribution mode for load balancer](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-distribution-mode). 


## Summary

This advanced networking scenarios guide was intended to help an Azure architect with the more complex integration of on-premises networks with Azure networking, which is a common requirement when 10, 20, or more workloads are deployed into Azure.  With a more robust network implementation, the organization can begin the task of migrating complex workloads such as SharePoint Server, Exchange Server, and SQL Server  to Azure, by using tools such as Azure Site Recovery as the replication and migration solution. 

## Next steps

- Review [Azure Site Recovery capacity planning guide](asr-capacity-planning.md)
- Review [Azure Site Recovery setup and configuration guide](asr-setup-guide.md)
