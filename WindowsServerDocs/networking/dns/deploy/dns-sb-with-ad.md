---
title: Usare i criteri DNS per DNS "split Brain" in Active Directory
description: You can use this topic to leverage traffic management capabilities of DNS policies for split-brain deployments with Active Directory integrated DNS zones in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9533204-ad7e-4e49-81c1-559324a16aeb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d294fcad3b48c8698dffd93e94f6ef7ffc681ea2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-split-brain-dns-in-active-directory"></a>Usare i criteri DNS per DNS "split Brain" in Active Directory

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

You can use this topic to leverage the traffic management capabilities of DNS policies for split\-brain deployments with Active Directory integrated DNS zones in Windows Server 2016.

In Windows Server 2016, DNS policies support is extended to Active Directory integrated DNS zones. Active Directory integration provides multi\-master high availability capabilities to the DNS server. 

In precedenza, questo scenario è necessario che gli amministratori DNS mantenere due diversi server DNS, ogni fornire servizi a ogni gruppo di utenti, interni ed esterni. Se solo alcuni record all'interno della zona sono stati se non Split o entrambe le istanze della zona (interna ed esterna) sono state delegate allo stesso dominio padre, questo è diventato un problema di gestione.

>[!NOTE]
> - DNS deployments are split\-brain when there are two versions of a single zone, one version for internal users on the organization intranet, and one version for external users – who are, typically, users on the Internet.
> - The topic [Use DNS Policy for Split-Brain DNS Deployment](split-brain-DNS-deployment.md) explains how you can use DNS policies and zone scopes to deploy a split\-brain DNS system on a single Windows Server 2016 DNS server.



##  <a name="example-split-brain-dns-in-active-directory"></a>Example Split\-Brain DNS in Active Directory

Questo esempio Usa una società fittizia, Contoso, che gestisce un sito Web di carriera in www.career.contoso.com.

Il sito dispone di due versioni, uno per gli utenti interni in cui sono disponibili le registrazioni processo interno. Questo sito interno è disponibile all'indirizzo IP locale 10.0.0.39. 

La seconda versione è la versione pubblica dello stesso sito, disponibile all'indirizzo IP pubblico 65.55.39.10.

In assenza di criteri DNS, l'amministratore è necessario per ospitare queste due zone in server DNS di Windows Server distinti e gestirli separatamente. 

Utilizzo dei criteri DNS queste aree possono essere ospitate ora nello stesso server DNS.

If the DNS server for contoso.com is Active Directory integrated, and is listening on two network interfaces, the Contoso DNS Administrator can follow the steps in this topic to achieve a split\-brain deployment.

The DNS Administrator configures the DNS server interfaces with the following IP addresses.

- The Internet facing network adapter is configured with a public IP address of 208.84.0.53 for external queries.
- The Intranet facing network adapter is configured with a private IP address of 10.0.0.56 for internal queries.

Nella figura seguente viene illustrato questo scenario.

![Split-Brain AD integrated DNS Deployment](../../media/DNS-SB-AD/DNS-SB-AD.jpg)

## <a name="how-dns-policy-for-split-brain-dns-in-active-directory-works"></a>How DNS Policy for Split\-Brain DNS in Active Directory Works

Quando il server DNS è configurato con i criteri necessari DNS, ogni richiesta di risoluzione dei nomi viene valutata rispetto ai criteri sul server DNS.

L'interfaccia server viene utilizzato in questo esempio come i criteri per distinguere i client interni ed esterni.

Se l'interfaccia del server su cui viene ricevuta la query corrisponde a uno qualsiasi dei criteri, l'ambito di zona associato viene utilizzato per rispondere alla query. 

Quindi, nel nostro esempio, le query DNS per www.career.contoso.com ricevuti IP privato (10.0.0.56) ricevano una risposta DNS che contiene un indirizzo IP interno. e le query DNS che vengono ricevute nell'interfaccia di rete pubblica ricevano una risposta DNS che contiene l'indirizzo IP pubblico nell'ambito di zona predefinito (si tratta dello stesso come la risoluzione di query normale).  

Support for Dynamic DNS \(DDNS\) updates and scavenging is supported only on the default zone scope. Poiché i client interni serviti dalla ambito predefinito zona, gli amministratori DNS Contoso continuare a utilizzare i meccanismi esistenti (dinamici DNS o statico) per aggiornare i record in contoso.com. Per gli ambiti di zona non predefinita \ (ad esempio, l'ambito esterno in questo example\), DDNS o scavenging supporto non è disponibile.

### <a name="high-availability-of-policies"></a>High Availability of policies

DNS policies are not Active Directory integrated. Because of this, DNS policies are not replicated to the other DNS servers that are hosting the same Active Directory integrated zone. 

DNS policies are stored on the local DNS server. You can easily export DNS policies from one server to another by using the following example Windows PowerShell commands.

    $policies = Get-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server01
    
    $policies |  Add-DnsServerQueryResolutionPolicy -ZoneName "contoso.com" -ComputerName Server02

For more information, see the following Windows PowerShell reference topics.

- [Get-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/itpro/powershell/windows/dns-server/get-dnsserverqueryresolutionpolicy)
- [Add-DnsServerQueryResolutionPolicy](https://technet.microsoft.com/itpro/powershell/windows/dns-server/add-dnsserverqueryresolutionpolicy)


## <a name="how-to-configure-dns-policy-for-split-brain-dns-in-active-directory"></a>How to Configure DNS Policy for Split\-Brain DNS in Active Directory

To configure DNS Split-Brain Deployment by using DNS Policy, you must use the following sections, which provide detailed configuration instructions.

### <a name="add-the-active-directory-integrated-zone"></a>Add the Active Directory integrated zone

You can use the following example command to add the Active Directory integrated contoso.com zone to the DNS server.

    Add-DnsServerPrimaryZone -Name "contoso.com" -ReplicationScope "Domain" -PassThru

For more information, see [Add-DnsServerPrimaryZone](https://technet.microsoft.com/library/jj649876.aspx).

### <a name="create-the-scopes-of-the-zone"></a>Creare gli ambiti della zona

You can use this section to partition the zone contoso.com to create an external zone scope.

Un ambito di zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona che contiene un proprio set di record DNS. Lo stesso record possono essere presenti in più ambiti, con indirizzi IP diversi o gli stessi indirizzi IP. 

Because you are adding this new zone scope in an Active Directory integrated zone, the zone scope and the records inside it will replicate via Active Directory to other replica servers in the domain.

By default, a zone scope exists in every DNS zone. Questo ambito di zona con lo stesso nome della zona e operazioni DNS legacy funzionano in questo ambito. This default zone scope will host the internal version of www.career.contoso.com.

You can use the following example command to create the zone scope on the DNS server.

    Add-DnsServerZoneScope -ZoneName "contoso.com" -Name "external"

Per ulteriori informazioni, vedere [Aggiungi DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx).

### <a name="add-records-to-the-zone-scopes"></a>Aggiungere record per gli ambiti di zona

The next step is to add the records representing the web server host into the two zone scopes- external and default \(for internal clients\). 

In the default internal zone scope, the record www.career.contoso.com is added with IP address 10.0.0.39, which is a private IP address; and in the external zone scope, the same record \(www.career.contoso.com\) is added with the public IP address 65.55.39.10. 

The records \(both in the default internal zone scope and the external zone scope\)  will automatically replicate across the domain with their respective zone scopes.

You can use the following example command to add records to the zone scopes on the DNS server.

    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "65.55.39.10" -ZoneScope "external"
    
    Add-DnsServerResourceRecord -ZoneName "contoso.com" -A -Name "www.career" -IPv4Address "10.0.0.39”

>[!NOTE]
>The **–ZoneScope** parameter is not included when the record is added to the default zone scope. This action is same as adding records to a normal zone.

Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).

### <a name="create-the-dns-policies"></a>Creare i criteri DNS

Dopo avere identificato le interfacce del server per la rete esterna e la rete interna e aver creato gli ambiti di zona, è necessario creare criteri DNS che si connettono gli ambiti di zona interni ed esterni.

>[!NOTE]
>This example uses the server interface \(the -ServerInterface parameter in the example command below\) as the criteria to differentiate between the internal and external clients. Un altro metodo per distinguere i client esterni e interni è utilizzando subnet del client come un criterio. Se è possibile identificare la subnet a cui appartengono i client interni, è possibile configurare criteri DNS per distinguere in base alle subnet del client. For information on how to configure traffic management using client subnet criteria, see [Use DNS Policy for Geo-Location Based Traffic Management with Primary Servers](primary-geo-location.md).

After you configure policies, when a DNS query is received on the public interface, the answer is returned from the external scope of the zone. 

>[!NOTE]
>No policies are required for mapping the default internal zone scope. 

    Add-DnsServerQueryResolutionPolicy -Name "SplitBrainZonePolicy" -Action ALLOW -ServerInterface "eq,208.84.0.53" -ZoneScope "external,1" -ZoneName contoso.com

>[!NOTE]
>208.84.0.53 is the IP address on the public network interface.

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).

Now the DNS server is configured with the required DNS policies for a split-brain name server with an Active Directory integrated DNS zone.

È possibile creare migliaia di criteri DNS in base del traffico di gestione, e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, su query in ingresso. 
