---
title: DNS Responses Based on Time of Day with an Azure Cloud App Server
description: In questo argomento fa parte di DNS criteri Scenario Guide per Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 4846b548-8fbc-4a7f-af13-09e834acdec0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3255d3fe29f6a7dda821183fa4352964cc230a5f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="dns-responses-based-on-time-of-day-with-an-azure-cloud-app-server"></a>DNS Responses Based on Time of Day with an Azure Cloud App Server

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

You can use this topic to learn how to distribute application traffic across different geographically distributed instances of an application by using DNS policies that are based on the time of day. 

This scenario is useful in situations where you want to direct traffic in one time zone to alternate application servers, such as Web servers that are hosted on Microsoft Azure, that are located in another time zone. This allows you to load balance traffic across application instances during peak time periods when your primary servers are overloaded with traffic. 

>[!NOTE]
>To learn how to use DNS policy for intelligent DNS responses without using Azure, see [Use DNS Policy for Intelligent DNS Responses Based on the Time of Day](Scenario--Use-DNS-Policy-for-Intelligent-DNS-Responses-Based-on-the-Time-of-Day.md). 

## <a name="bkmk_azureexample"></a>Example of Intelligent DNS Responses Based on the Time of Day with Azure Cloud App Server

Following is an example of how you can use DNS policy to balance application traffic based on the time of day.

This example uses one fictional company, Contoso Gift Services, which provides online gifting solutions across the globe through their Web site, contosogiftservices.com. 

The contosogiftservices.com web site is hosted only at a single on-premises datacenter in Seattle (with public IP 192.68.30.2). 

The DNS server is also located in the on-premises datacenter. 

With a recent surge in business, contosogiftservices.com has a higher number of visitors every day, and some of the customers have reported service availability issues. 

Contoso Gift Services performs a site analysis, and discovers that every evening between 6 PM and 9 PM local time, there is a surge in the traffic to the Seattle Web server. The Web server cannot scale to handle the increased traffic at these peak hours, resulting in denial of service to customers. 

To ensure that contosogiftservices.com customers get a responsive experience from the Web site, Contoso Gift Services decides that during these hours it will rent a virtual machine \(VM\) on Microsoft Azure to host a copy of its Web server.  

Contoso Gift Services gets a public IP address from Azure for the VM (192.68.31.44) and develops the automation to deploy the Web Server every day on Azure between 5-10 PM, allowing for a one hour contingency period.

>[!NOTE]
>For more information about Azure VMs, see [Virtual Machines documentation](https://azure.microsoft.com/documentation/services/virtual-machines/) 

The DNS servers are configured with zone scopes and DNS policies so that between 5-9 PM every day, 30% of queries are sent to the instance of the Web server that is running in Azure.

Nella figura seguente viene illustrato questo scenario.

![DNS Policy for time of day responses](../../media/DNS-Policy-Tod2/dns_policy_tod2.jpg)  

## <a name="bkmk_azurehow"></a>How Intelligent DNS Responses Based on Time of Day with Azure App Server Works
 
This article demonstrates how to configure the DNS server to answer DNS queries with two different application server IP addresses - one web server is in Seattle and the other is in an Azure datacenter.

After the configuration of a new DNS policy that is based on the peak hours of 6 PM to 9 PM in Seattle, the DNS server sends seventy per cent of the DNS responses to clients containing the IP address of the Seattle Web server, and thirty per cent of the DNS responses to clients containing the IP address of the Azure Web server, thereby directing client traffic to the new Azure Web server, and preventing the Seattle Web server from becoming overloaded. 

At all other times of day, the normal query processing takes place and responses are sent from default zone scope which contains a record for the web server in the on-premises datacenter. 

The TTL of 10 minutes on the Azure record ensures that the record is expired from the LDNS cache before the VM is removed from Azure. One of the benefits of such scaling is that you can keep your DNS data on-premises, and keep scaling out to Azure as demand requires.

## <a name="bkmk_azureconfigure"></a>How to Configure DNS Policy for Intelligent DNS Responses Based on Time of Day with Azure App Server
To configure DNS policy for time of day application load balancing based query responses, you must perform the following steps.


- [Creare gli ambiti di zona](#bkmk_zscopes)
- [Aggiungere record per gli ambiti di zona](#bkmk_records)
- [Creare i criteri DNS](#bkmk_policies)


>[!NOTE]
>È necessario eseguire questi passaggi nel server DNS autorevole per la zona in cui che si desidera configurare. Membership in DnsAdmins, or equivalent, is required to perform the following procedures. 

Le sezioni seguenti forniscono istruzioni dettagliate di configurazione.

>[!IMPORTANT]
>Le sezioni seguenti includono esempi di comandi Windows PowerShell che contengono i valori di esempio per numero di parametri. Assicurarsi di sostituire i valori di esempio in questi comandi con i valori appropriati per la distribuzione prima di eseguire questi comandi. 


### <a name="bkmk_zscopes"></a>Creare gli ambiti di zona
Un ambito di zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona che contiene un proprio set di record DNS. Lo stesso record possono essere presenti in più ambiti, con indirizzi IP diversi o gli stessi indirizzi IP. 

>[!NOTE]
>Per impostazione predefinita, un ambito di una zona esistente nelle zone DNS. Questo ambito di zona con lo stesso nome della zona e operazioni DNS legacy funzionano in questo ambito. 

You can use the following example command to create a zone scope to host the Azure records.

```
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AzureZoneScope"
```

Per ulteriori informazioni, vedere [Aggiungi DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

### <a name="bkmk_records"></a>Aggiungere record per gli ambiti di zona
The next step is to add the records representing the Web server host into the zone scopes. 

In AzureZoneScope, the record www.contosogiftservices.com is added with IP address 192.68.31.44, which is located in the Azure public cloud. 

Similarly, in the default zone scope \(contosogiftservices.com\), a record \(www.contosogiftservices.com\) is added with IP address 192.68.30.2 of the Web server running in the Seattle on-premises datacenter.

In the second cmdlet below, the –ZoneScope parameter is not included. Because of this,  the records are added in the default ZoneScope. 

In addition, the TTL of the record for Azure VMs is kept at 600s (10 mins) so that the LDNS do not cache it for a longer time - which would interfere with load balancing. Also, the Azure VMs are available for 1 extra hour as a contingency to ensure that even clients with cached records are able to resolve.

```
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.31.44" -ZoneScope "AzureZoneScope" –TimeToLive 600

Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.68.30.2"
```

Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).  

### <a name="bkmk_policies"></a>Creare i criteri DNS 
After the zone scopes are created, you can create DNS policies that distribute the incoming queries across these scopes so that the following occurs.

1. From 6 PM to 9 PM daily, 30% of clients receive the IP address of the Web server in the Azure datacenter in the DNS response, while 70% of clients receive the IP address of the Seattle on-premises Web server.
2. At all other times, all the clients receive the IP address of the Seattle on-premises Web server.

The time of the day has to be expressed in local time of the DNS server.

You can use the following example command to create the DNS policy.

```
Add-DnsServerQueryResolutionPolicy -Name "Contoso6To9Policy" -Action ALLOW –-ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” -ZoneName "contosogiftservices.com" –ProcessingOrder 1
```

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).  
  
Now the DNS server is configured with the required DNS policies to redirect traffic to the Azure Web server based on time of day. 

Note the expression:

`
 -ZoneScope "contosogiftservices.com,7;AzureZoneScope,3" –TimeOfDay “EQ,18:00-21:00” 
`

This expression configures the DNS server with a ZoneScope and weight combination that instructs the DNS server to send the IP address of the Seattle Web server seventy per cent of the time, while sending the IP address of the Azure Web server thirty per cent of the time.

È possibile creare migliaia di criteri DNS in base del traffico di gestione, e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, su query in ingresso.
