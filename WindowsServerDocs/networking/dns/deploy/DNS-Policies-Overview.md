---
title: Cenni preliminari sui criteri DNS
description: In questo argomento fa parte di DNS criteri Scenario Guide per Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 566bc270-81c7-48c3-a904-3cba942ad463
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 06086bbd7edc2fa489805eb5075062332e002ab4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="dns-policies-overview"></a>Cenni preliminari sui criteri DNS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

You can use this topic to learn about DNS Policy, which is new in Windows Server 2016. È possibile utilizzare criteri DNS per la gestione del traffico basato su posizione geografica, risposte DNS basate sull'ora del giorno, per gestire un singolo server DNS configurato per la distribuzione "split Brain", l'applicazione di filtri query DNS e altro ancora. Gli elementi seguenti forniscono altri dettagli su queste funzionalità.

-   **Bilanciamento carico di applicazioni.** Quando si distribuiscono più istanze di un'applicazione in posizioni diverse, è possibile utilizzare criteri DNS per bilanciare il carico del traffico tra le istanze di applicazioni diverso, allocazione dinamica il carico del traffico per l'applicazione.

-   **Gestione del traffico basata sulla Geo\-Location.** È possibile utilizzare criteri DNS per consentire ai server DNS primari e secondari rispondere alle query client DNS in base alla posizione geografica del client e la risorsa a cui il client sta tentando di connettersi, fornendo il client con l'indirizzo IP della risorsa più vicina. 

-   **Dividere cervello DNS.** Con DNS "split Brain", i record DNS sono suddivisi in diversi ambiti di zona nello stesso server DNS e client DNS ricevano una risposta basata sul fatto che i client si trovano i client interni o esterni. È possibile configurare DNS "split Brain" per le zone integrate con Active Directory o per le zone in server DNS autonomi.

-   **Il filtro.** È possibile configurare criteri DNS per creare i filtri di query basate su criteri specificati. Filtri di query in Criteri di DNS consentono di configurare il server DNS per rispondere in modo personalizzato in base alla query DNS e client DNS che invia la query DNS. 
-   **Analisi forense.** È possibile utilizzare criteri DNS per reindirizzare i client DNS non autorizzati a un indirizzo IP esistente anziché al computer che stanno tentando di raggiungere.

-   **Ora del giorno in base reindirizzamento.** È possibile utilizzare criteri DNS per distribuire il traffico dell'applicazione tra diverse istanze distribuite geograficamente di un'applicazione utilizzando i criteri DNS basate sull'ora del giorno.

## <a name="new-concepts"></a>New Concepts  
In order to create policies to support the scenarios listed above, it is necessary to be able to identify groups of records in a zone, groups of clients on a network, among other elements. These elements are represented by the following new DNS objects:  

- **Client subnet:** a client subnet object represents an IPv4 or IPv6 subnet from which queries are submitted to a DNS server. You can create subnets to later define policies to be applied based on what subnet the requests come from. For instance, in a split brain DNS scenario, the request for resolution for a name such as *www.microsoft.com* can be answered with an internal IP address to clients from internal subnets, and a different IP address to clients in external subnets.

- **Recursion scope:** recursion scopes are unique instances of a group of settings that control recursion on a DNS server. Un ambito di ricorsione contiene un elenco di server d'inoltro e specifica se è abilitata la ricorsione. Un server DNS può avere molti gli ambiti di ricorsione. DNS server recursion policies allow you to choose a recursion scope for a set of queries. If the DNS server is not authoritative for certain queries, DNS server recursion policies allow you to control how to resolve those queries. You can specify which forwarders to use and whether to use recursion.

- **Zone scopes:** a DNS zone can have multiple zone scopes, with each zone scope containing their own set of DNS records. The same record can be present in multiple scopes, with different IP addresses. Also, zone transfers are done at the zone scope level. That means that records from a zone scope in a primary zone will be transferred to the same zone scope in a secondary zone.

## <a name="types-of-policy"></a>Types of Policy

DNS Policies are divided by level and type. You can use Query Resolution Policies to define how queries are processed, and Zone Transfer Policies to define how zone transfers occur. You can apply Each policy type at the server level or the zone level.
  
### <a name="query-resolution-policies"></a>Query Resolution Policies

You can use DNS Query Resolution Policies to specify how incoming resolution queries are handled by a DNS server. Every DNS Query Resolution Policy contains the following elements:  
  
|Field|Descrizione|Possible values|  
|---------|---------------|-------------------|  
|**Nome**|Policy name|-   Up to 256 characters<br />-   Can contain any character valid for a file name|  
|**State**|Policy state|-   Enable (default)<br />-   Disabled|  
|**Level**|Policy level|-   Server<br />-   Zone|  
|**Processing order**|Once a query is classified by level and applies on, the server finds the first policy for which the query matches the criteria and applies it to query|-   Numeric value<br />-   Unique value per policy containing the same level and applies on value|  
|**Azione**|Action to be performed by DNS server|-   Allow (default for zone level)<br />-   Deny (default on server level)<br />-   Ignore|  
|**Criteria**|Policy condition (AND/OR) and list of criterion to be met for policy to be applied|-   Condition operator (AND/OR)<br />-   List of criteria (see the criterion table below)|  
|**Ambito**|List of zone scopes and weighted values per scope. Weighted values are used for load balancing distribution. For instance, if this list includes datacenter1 with a weight of 3 and datacenter2 with a weight of 5 the server will respond with a record from datacentre1 three times out of eight requests|-   List of zone scopes (by name) and weights|  

> [!NOTE]
> Server level policies can only have the values **Deny** or **Ignore** as an action.

The DNS policy criteria field is composed of two elements:

|Nome|Descrizione|Sample values|
|--------|---------------|-----------------|
|**Subnet del client**|Il trasporto di protocollo utilizzato nella query. Possible entries are **UDP** and **TCP**|-   **EQ,Spain,France** - resolves to true if the subnet is identified as either Spain or France<br />-   **NE,Canada,Mexico** - resolves to true if the client subnet is any subnet other than Canada and Mexico|  
|**Protocollo di trasporto**|Il trasporto di protocollo utilizzato nella query. Possible entries are **UDP** and **TCP**|-   **EQ,TCP**<br />-   **EQ,UDP**|  
|**Protocollo Internet**|Protocollo di rete utilizzato nella query. Possible entries are **IPv4** and **IPv6**|-   **EQ,IPv4**<br />-   **EQ,IPv6**|  
|**Indirizzo IP dell'interfaccia del server**|IP address for the incoming DNS server network interface|-   **EQ,10.0.0.1**<br />-   **EQ,192.168.1.1**|  
|**NOME DI DOMINIO COMPLETO**|FQDN of record in the query, with the possibility of using a wild card|-   **EQ,www.contoso.com** - resolves tot rue only the if the query is trying to resolve the *www.contoso.com* FQDN<br />-   **EQ,\*.contoso.com,\*.woodgrove.com** - resolves to true if the query is for any record ending in *contoso.com***OR***woodgrove.com*|  
|**Tipo di query**|Type of record being queried (A, SVR, TXT)|-   **EQ,TXT,SRV** - resolves tot rue if the query is requesting a TXT **OR** SRV record<br />-   **EQ,MX** - resolves tot rue if the query is requesting an MX record|  
|**Ora del giorno**|Time of day the query is received|-   **EQ,10:00-12:00,22:00-23:00** - resolves tot rue if the query is received between 10 AM and noon, **OR** between 10PM and 11PM|  
  
Using the table above as a starting point, the table below could be used to define a criterion that is used to match queries for any type of records but SRV records in the contoso.com domain coming from a client in the 10.0.0.0/24 subnet via TCP between 8 and 10 PM through interface 10.0.0.3:  
  
|Nome|Valore|  
|--------|---------|  
|Subnet del client|EQ,10.0.0.0/24|  
|Protocollo di trasporto|EQ,TCP|  
|Indirizzo IP dell'interfaccia del server|EQ,10.0.0.3|  
|NOME DI DOMINIO COMPLETO|EQ,*.contoso.com|  
|Tipo di query|NE,SRV|  
|Ora del giorno|EQ,20:00-22:00|  
  
You can create multiple query resolution policies of the same level, as long as they have a different value for the processing order. When multiple policies are available, the DNS server processes incoming queries in the following manner:  
  
![DNS policy processing](../../media/DNS-Policies-Overview/DNSQueryResolutionPolicyFlowchart.png)  
  
### <a name="recursion-policies"></a>Recursion Policies  
Recursion policies are a special **type** of server level policies. Recursion policies control how the DNS server performs recursion for a query. Recursion policies apply only when query processing reaches the recursion path. You can choose a value of DENY or IGNORE for recursion for a set of queries. Alternatively, you can choose a set of forwarders for a set of queries.  
  
You can use recursion policies to implement a Split-brain DNS configuration. In this configuration, the DNS server performs recursion for a set of clients for a query, while the DNS server does not perform recursion for other clients for that query.  
  
Recursion policies contains the same elements a regular DNS query resolution policy contains, along with the elements in the table below:  
  
|Nome|Descrizione|  
|--------|---------------|  
|**Apply on recursion**|Specifies that this policy should only be used for recursion.|  
|**Recursion Scope**|Name of the recursion scope.|  
  
> [!NOTE]  
> Recursion policies can only be created at the server level.  
  
### <a name="zone-transfer-policies"></a>Zone Transfer Policies  
Zone transfer policies control whether a zone transfer is allowed or not by your DNS server. You can create policies for zone transfer at either the server level or the zone level. Server level policies apply on every zone transfer query that occurs on the DNS server. Zone level policies apply only on the queries on a zone hosted on the DNS server. The most common use for zone level policies is to implement blocked or safe lists.  
  
> [!NOTE]  
> Zone transfer policies can only use DENY or IGNORE as actions.  
  
You can use the server level zone transfer policy below to deny a zone transfer for the contoso.com domain from a given subnet:  
  
```  
Add-DnsServerZoneTransferPolicy -Name DenyTransferOfCOnsotostoFabrikam -Zone contoso.com -Action DENY -ClientSubnet "EQ,192.168.1.0/24"  
```  
  
You can create multiple zone transfer policies of the same level, as long as they have a different value for the processing order. When multiple policies are available, the DNS server processes incoming queries in the following manner:  
  
![DNS process for multiple zone transfer policies](../../media/DNS-Policies-Overview/DNSPolicyZone.png)  
  
## <a name="managing-dns-policies"></a>Managing DNS Policies  
You can create and manage DNS Policies by using PowerShell. The examples below go through different sample scenarios that you can configure through DNS Policies:  
  
### <a name="traffic-management"></a>Traffic Management  
You can direct traffic based on an FQDN to different servers depending on the location of the DNS client. The example below shows how to create traffic management policies to direct the customers from a certain subnet to a North American datacenter and from another subnet to a European datacenter.  
  
```  
Add-DnsServerClientSubnet -Name "NorthAmericaSubnet" -IPv4Subnet "172.21.33.0/24"  
Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet "172.17.44.0/24"  
Add-DnsServerZoneScope -ZoneName "Contoso.com" -Name "NorthAmericaZoneScope"  
Add-DnsServerZoneScope -ZoneName "Contoso.com" -Name "EuropeZoneScope"  
Add-DnsServerResourceRecord -ZoneName "Contoso.com" -A -Name "www" -IPv4Address "172.17.97.97" -ZoneScope "EuropeZoneScope"  
Add-DnsServerResourceRecord -ZoneName "Contoso.com" -A -Name "www" -IPv4Address "172.21.21.21" -ZoneScope "NorthAmericaZoneScope"  
Add-DnsServerQueryResolutionPolicy -Name "NorthAmericaPolicy" -Action ALLOW -ClientSubnet "eq,NorthAmericaSubnet" -ZoneScope "NorthAmericaZoneScope,1" -ZoneName "Contoso.com"  
Add-DnsServerQueryResolutionPolicy -Name "EuropePolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "EuropeZoneScope,1" -ZoneName contoso.com  
```  
  
The first two lines of the script create client subnet objects for North America and Europe. The two lines after that create a zone scope within the contoso.com domain, one for each region. The two lines after that create a record in each zone that associates ww.contoso.com to different IP address, one for Europe, another one for North America. Finally, the last lines of the script create two DNS Query Resolution Policies, one to be applied to the North America subnet, another to the Europe subnet.  
  
### <a name="block-queries-for-a-domain"></a>Block queries for a domain  
You can use a DNS Query Resolution Policy to block queries to a domain. The example below blocks all queries to treyresearch.net:  
  
```  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicy" -Action IGNORE -FQDN "EQ,*.treyresearch.com"  
```  
  
### <a name="block-queries-from-a-subnet"></a>Block queries from a subnet  
You can also block queries coming from a specific subnet. The script below creates a subnet for 172.0.33.0/24 and then creates a policy to ignore all queries coming from that subnet:  
  
```  
Add-DnsServerClientSubnet -Name "MaliciousSubnet06" -IPv4Subnet 172.0.33.0/24  
Add-DnsServerQueryResolutionPolicy -Name "BlackholePolicyMalicious06" -Action IGNORE -ClientSubnet  "EQ,MaliciousSubnet06"  
```  
  
### <a name="allow-recursion-for-internal-clients"></a>Allow recursion for internal clients  
È possibile controllare la ricorsione utilizzando un criterio di risoluzione di Query DNS. L'esempio seguente consente di abilitare la ricorsione per i client interni, disattivarlo per i client esterni in uno scenario di cervello split.  
  
```  
Set-DnsServerRecursionScope -Name . -EnableRecursion $False   
Add-DnsServerRecursionScope -Name "InternalClients" -EnableRecursion $True  
Add-DnsServerQueryResolutionPolicy -Name "SplitBrainPolicy" -Action ALLOW -ApplyOnRecursion -RecursionScope "InternalClients" -ServerInterfaceIP  "EQ,10.0.0.34"  
```  
  
La prima riga nello script modifica l'ambito di ricorsione predefinito, denominato semplicemente come "." (punto) per disattivare la ricorsione. La seconda riga crea un ambito di ricorsione denominato *InternalClients* con ricorsione abilitata. E la terza riga crea un criterio per applicare l'appena creare l'ambito di ricorsione per le query in entrata tramite un'interfaccia di server che ha 10.0.0.34 come un indirizzo IP.  
  
### <a name="create-a-server-level-zone-transfer-policy"></a>Creare un criterio di trasferimento di zona livello di server  
È possibile controllare il trasferimento di zona in un formato più granulare tramite criteri di trasferimento di zona DNS. Per consentire i trasferimenti di zona per qualsiasi server in una determinata subnet, è possibile utilizzare lo script di esempio seguente:  
  
```  
Add-DnsServerClientSubnet -Name "AllowedSubnet" -IPv4Subnet 172.21.33.0/24  
Add-DnsServerZoneTransferPolicy -Name "NorthAmericaPolicy" -Action IGNORE -ClientSubnet "ne,AllowedSubnet"  
```  
  
La prima riga nello script crea un oggetto subnet denominato *AllowedSubnet* con l'IP bloccare 172.21.33.0/24. La seconda riga crea un criterio di trasferimento di zona per consentire i trasferimenti di zona per qualsiasi server DNS nella subnet creato in precedenza.  
  
### <a name="create-a-zone-level-zone-transfer-policy"></a>Creare un criterio di trasferimento zona livello di zona  
È inoltre possibile creare criteri di trasferimento di zona livello di zona. L'esempio seguente ignora qualsiasi richiesta per un trasferimento di zona per contoso.com provenienti da un'interfaccia di server che dispone di un indirizzo IP di 10.0.0.33:  
  
```  
Add-DnsServerZoneTransferPolicy -Name "InternalTransfers" -Action IGNORE -ServerInterfaceIP "ne,10.0.0.33" -PassThru -ZoneName "contoso.com"  
```  
  
## <a name="dns-policy-scenarios"></a>Scenari di criteri DNS

Per informazioni su come usare i criteri DNS per scenari specifici, vedere gli argomenti seguenti in questa Guida.

- [Usare i criteri DNS per posizione geografica basato su gestione del traffico con i server primari](primary-geo-location.md)  
- [Usare i criteri DNS per posizione geografica basato su gestione del traffico con distribuzioni primario secondario](primary-secondary-geo-location.md)  
- [Use DNS Policy for Intelligent DNS Responses Based on the Time of Day](dns-tod-intelligent.md)
- [DNS Responses Based on Time of Day with an Azure Cloud App Server](dns-tod-azure-cloud-app-server.md)
- [Usare i criteri DNS per la distribuzione DNS "split Brain"](split-brain-DNS-deployment.md)
- [Usare i criteri DNS per DNS "split Brain" in Active Directory](dns-sb-with-ad.md)
- [Usare i criteri DNS per l'applicazione di filtri alle query DNS](apply-filters-on-dns-queries.md)
- [Usare i criteri DNS per Bilanciamento carico di applicazioni](app-lb.md)
- [Usare i criteri DNS per l'applicazione bilanciamento del carico con la consapevolezza della posizione geografica](app-lb-geo.md)


