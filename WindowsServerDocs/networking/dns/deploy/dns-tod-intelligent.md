---
title: Use DNS Policy for Intelligent DNS Responses Based on the Time of Day
description: In questo argomento fa parte di DNS criteri Scenario Guide per Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 161446ff-a072-4cc4-b339-00a04857ff3a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 46821daff4a0046bf78d7f56dc7c5deabcc437e4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-intelligent-dns-responses-based-on-the-time-of-day"></a>Use DNS Policy for Intelligent DNS Responses Based on the Time of Day

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

You can use this topic to learn how to distribute application traffic across different geographically distributed instances of an application by using DNS policies that are based on the time of day.  
  
This scenario is useful in situations where you want to direct traffic in one time zone to alternate application servers, such as Web servers, that are located in another time zone. This allows you to load balance traffic across application instances during peak time periods when your primary servers are overloaded with traffic.   
  
### <a name="bkmk_example1"></a>Example of Intelligent DNS Responses Based on the Time of Day  
Following is an example of how you can use DNS policy to balance application traffic based on the time of day.  
  
This example uses one fictional company, Contoso Gift Services, which provides online gifting solutions across the globe through their Web site, contosogiftservices.com.   
  
The contosogiftservices.com Web site is hosted in two datacenters, one in Seattle (North America) and another in Dublin (Europe). The DNS servers are configured for sending geo-location aware responses using DNS policy. With a recent surge in business, contosogiftservices.com has a higher number of visitors every day, and some of the customers have reported service availability issues.  
  
Contoso Gift Services performs a site analysis, and discovers that every evening between 6 PM and 9 PM local time, there is a surge in the traffic to the Web servers. The Web servers cannot scale to handle the increased traffic at these peak hours, resulting in denial of service to customers. The same peak hour traffic overload happens in both the European and American datacenters. At other times of day, the servers handle traffic volumes that are well below their maximum capability.  
  
To ensure that contosogiftservices.com customers get a responsive experience from the Web site, Contoso Gift Services wants to redirect some Dublin traffic to the Seattle application servers between 6 PM and 9 PM in Dublin; and they want to redirect some Seattle traffic to the Dublin application servers between 6 PM and 9 PM in Seattle.  
  
Nella figura seguente viene illustrato questo scenario.  
  
![Time of Day DNS Policy example](../../media/DNS-Policy-Tod1/dns_policy_tod1.jpg)  
  
### <a name="bkmk_works1"></a>How Intelligent DNS Responses Based on Time of Day Works  
  
When the DNS server is configured with time of day DNS policy, between 6 PM and 9 PM at each geographical location, the DNS server does the following.  
  
- Answers the first four queries it receives with the IP address of the Web server in the local datacenter.  
- Answers the fifth query it receives with the IP address of the Web server in the remote datacenter.   
  
This policy-based behavior offloads twenty per cent of the local Web server's traffic load to the remote Web server, easing the strain on the local application server and improving site performance for customers.  
  
During off-peak hours, the DNS servers perform normal geo-locations based traffic management. In addition, DNS clients that send queries from locations other than North America or Europe, the DNS server load balances the traffic across the Seattle and Dublin datacenters.  
  
When multiple DNS policies are configured in DNS, they are an ordered set of rules, and they are processed by DNS from highest priority to lowest priority. DNS uses the first policy that matches the circumstances, including time of day. For this reason, more specific policies should have higher priority. If you create time of day policies and give them high priority in the list of policies, DNS processes and uses these policies first if they match the parameters of the DNS client query and the criteria defined in the policy. If they don't match, DNS moves down the list of policies to process the default policies until it finds a match.  
  
For more information about policy types and criteria, see [DNS Policies Overview](../../dns/deploy/DNS-Policies-Overview.md).  
  
### <a name="bkmk_how1"></a>How to Configure DNS Policy for Intelligent DNS Responses Based on Time of Day  
To configure DNS policy for time of day application load balancing based query responses, you must perform the following steps.  
  
- [Creare la subnet del Client DNS](#bkmk_subnets)  
- [Creare gli ambiti di zona](#bkmk_zscopes)  
- [Aggiungere record per gli ambiti di zona](#bkmk_records)  
- [Creare i criteri DNS](#bkmk_policies)  
  
>[!NOTE]
>È necessario eseguire questi passaggi nel server DNS autorevole per la zona in cui che si desidera configurare. Appartenenza al gruppo **DnsAdmins**, o equivalente, è necessario eseguire le procedure seguenti.  
  
Le sezioni seguenti forniscono istruzioni dettagliate di configurazione.  
  
>[!IMPORTANT]
>Le sezioni seguenti includono esempi di comandi Windows PowerShell che contengono i valori di esempio per numero di parametri. Assicurarsi di sostituire i valori di esempio in questi comandi con i valori appropriati per la distribuzione prima di eseguire questi comandi.  
  
#### <a name="bkmk_subnets"></a>Creare la subnet del Client DNS  
Il primo passaggio consiste nell'identificare le subnet o spazio di indirizzi IP delle aree per cui si desidera reindirizzare il traffico. Ad esempio, se si desidera reindirizzare il traffico per Stati Uniti ed Europa, è necessario identificare la subnet o spazi degli indirizzi IP di queste aree.  
  
È possibile ottenere queste informazioni da mappe Geo-IP. In base a queste distribuzioni geografica IP, è necessario creare "DNS Client subnet". Una Subnet del Client DNS è un raggruppamento logico di subnet IPv4 o IPv6 da cui le query vengono inviate a un server DNS.  
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per creare subnet del Client DNS.  
  
```  
Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet "192.0.0.0/24, 182.0.0.0/24"  
  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "141.1.0.0/24, 151.1.0.0/24"  
  
```  
Per ulteriori informazioni, vedere [Aggiungi DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).  
  
#### <a name="bkmk_zscopes"></a>Creare gli ambiti di zona  
Dopo avere configurata la subnet del client, è necessario partizionare la zona il cui traffico che si desidera reindirizzare in due ambiti fuso diverso, un ambito per ogni subnet Client DNS che è stato configurato.  
  
For example, if you want to redirect traffic for the DNS name www.contosogiftservices.com, you must create two different zone scopes in the contosogiftservices.com zone, one for the U.S. and one for Europe.  
  
Un ambito di zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona che contiene un proprio set di record DNS. Lo stesso record possono essere presenti in più ambiti, con indirizzi IP diversi o gli stessi indirizzi IP.  
  
>[!NOTE]
>Per impostazione predefinita, un ambito di una zona esistente nelle zone DNS. Questo ambito di zona con lo stesso nome della zona e operazioni DNS legacy funzionano in questo ambito.  
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per creare ambiti di zona.  
  
```  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"  
  
Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"  
  
```  
Per ulteriori informazioni, vedere [Aggiungi DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps).  
  
#### <a name="bkmk_records"></a>Aggiungere record per gli ambiti di zona  
Ora è necessario aggiungere i record che rappresenta l'host del server web in ambiti due zone.  
  
For example, in **SeattleZoneScope**, the record **www.contosogiftservices.com** is added with IP address 192.0.0.1, which is located in a Seattle datacenter. Similarly, in **DublinZoneScope**, the record **www.contosogiftservices.com** is added with IP address 141.1.0.3 in the Dublin datacenter  
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per aggiungere record per gli ambiti di zona.  
  
```  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope  
  
Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.3" -ZoneScope "DublinZoneScope"  
  
```  
The ZoneScope parameter is not included when you add a record in the default scope. Questo è uguale all'aggiunta di record per una zona DNS standard.  
  
Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).  
  
#### <a name="bkmk_policies"></a>Creare i criteri DNS  
Dopo aver creato le subnet, le partizioni (ambiti zona) ed è stato aggiunto record, è necessario creare criteri che si connettono le subnet e partizioni, in modo che quando una query provenga da un'origine in una subnet client DNS, la risposta alla query viene restituita dall'ambito corretto della zona. Criteri non sono necessari per il mapping di ambito predefinito zona.  
  
After you configure these DNS policies, the DNS server behavior is as follows:  
  
1. European DNS clients receive the IP address of the Web server in the Dublin datacenter in their DNS query response.  
2. American DNS clients receive the IP address of the Web server in the Seattle datacenter in their DNS query response.  
3. Between 6 PM and 9 PM in Dublin, 20% of the queries from European clients receive the IP address of the Web server in the Seattle datacenter in their DNS query response.  
4. Between 6 PM and 9 PM in Seattle, 20% of the queries from the American clients receive the IP address of the Web server in the Dublin datacenter in their DNS query response.  
5. Half of the queries from the rest of the world receive the IP address of the Seattle datacenter and the other half receive the IP address of the Dublin datacenter.  
  
  
È possibile utilizzare i seguenti comandi di Windows PowerShell per creare un criterio DNS che collega la subnet del Client DNS e gli ambiti di zona.  
  
>[!NOTE]
>In this example, the DNS server is in the GMT time zone, so the peak hour time periods must be expressed in the equivalent GMT time.  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "America6To9Policy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,4;DublinZoneScope,1" -TimeOfDay "EQ,01:00-04:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 1  
  
Add-DnsServerQueryResolutionPolicy -Name "Europe6To9Policy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "SeattleZoneScope,1;DublinZoneScope,4" -TimeOfDay "EQ,17:00-20:00" -ZoneName "contosogiftservices.com" -ProcessingOrder 2  
  
Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3  
  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 4  
  
Add-DnsServerQueryResolutionPolicy -Name "RestOfWorldPolicy" -Action ALLOW --ZoneScope "DublinZoneScope,1;SeattleZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 5  
  
```  
Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  
  
Now the DNS server is configured with the required DNS policies to redirect traffic based on geo-location and time of day.  
  
Quando il server DNS riceve una query di risoluzione dei nomi, il server DNS valuta i campi nella richiesta DNS con i criteri DNS configurati. Se l'indirizzo IP di origine nella richiesta di risoluzione nome corrisponde a uno qualsiasi dei criteri, l'ambito delle zone associati verrà utilizzato per rispondere alla query e l'utente viene indirizzato alla risorsa che geograficamente più vicino.  
  
È possibile creare migliaia di criteri DNS in base del traffico di gestione, e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, su query in ingresso.


