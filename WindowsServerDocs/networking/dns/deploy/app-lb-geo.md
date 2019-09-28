---
title: Usare i criteri DNS per l'applicazione del bilanciamento del carico con la consapevolezza della posizione geografica
description: Questo argomento fa parte della Guida allo scenario dei criteri DNS per Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ea3f959612de0f2bc56a887ba73aba47f1d3f141
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406214"
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>Usare i criteri DNS per l'applicazione del bilanciamento del carico con la consapevolezza della posizione geografica

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per informazioni su come configurare i criteri DNS per bilanciare il carico di un'applicazione con la consapevolezza della posizione geografica.

Nell'argomento precedente di questa guida, [usare i criteri DNS per il bilanciamento del carico dell'applicazione](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb), viene usato un esempio di un servizio fittizio aziendale-contoso Gift Services, che fornisce servizi di gifting online e che dispone di un sito Web denominato contosogiftservices.com. Contoso Gift Services bilancia il carico dell'applicazione Web online tra i server nei data center nordamericani situati a Seattle, WA, Chicago, IL e Dallas, TX.

>[!NOTE]
>È consigliabile acquisire familiarità con l'argomento [usare i criteri DNS per il bilanciamento del carico dell'applicazione](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb) prima di eseguire le istruzioni in questo scenario.

Questo argomento usa la stessa società fittizia e l'infrastruttura di rete come base per una nuova distribuzione di esempio che include la consapevolezza della posizione geografica.

In questo esempio contoso Gift Services sta espandendo la loro presenza in tutto il mondo.

Analogamente a America del Nord, la società dispone ora di server Web ospitati nei data center europei.

Gli amministratori DNS di Contoso Gift Services vogliono configurare il bilanciamento del carico delle applicazioni per i data center europei in modo analogo all'implementazione dei criteri DNS nel Stati Uniti, con il traffico delle applicazioni distribuito tra i server Web che si trovano in Dublino, Irlanda, Amsterdam, Olanda e altrove.

Gli amministratori DNS vogliono anche che tutte le query provenienti da altre località del mondo siano distribuite equamente tra tutti i rispettivi Data Center.

Nelle sezioni successive è possibile apprendere come raggiungere gli obiettivi simili a quelli degli amministratori DNS di Contoso nella propria rete.

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>Come configurare il bilanciamento del carico dell'applicazione con la consapevolezza della posizione geografica

Le sezioni seguenti illustrano come configurare i criteri DNS per il bilanciamento del carico dell'applicazione con la consapevolezza della posizione geografica.

>[!IMPORTANT]
>Nelle sezioni seguenti includono esempi di comandi Windows PowerShell che contengono valori di esempio per numero di parametri. Assicurarsi di sostituire i valori di esempio in questi comandi con i valori appropriati per la distribuzione prima di eseguire questi comandi.

### <a name="bkmk_clientsubnets"></a>Creare le subnet del client DNS

È necessario innanzitutto identificare le subnet o lo spazio di indirizzi IP delle aree America del Nord ed Europa.

È possibile ottenere queste informazioni da mappe Geo-IP. In base a queste distribuzioni geografiche IP, è necessario creare le subnet del client DNS.

Una Subnet del Client DNS è un raggruppamento logico di subnet IPv4 o IPv6 da cui le query vengono inviate a un server DNS.

È possibile utilizzare i seguenti comandi di Windows PowerShell per creare subnet del Client DNS. 

    
    Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
    
Per ulteriori informazioni, vedere [Aggiungi DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).

### <a name="bkmk_zscopes2"></a>Creare gli ambiti di zona

Dopo aver apportato le subnet client, è necessario partizionare la zona contosogiftservices.com in ambiti di zona diversi, ognuno per un Data Center.

Un ambito di una zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona contenente il proprio set di record DNS. Lo stesso record può essere presente in più ambiti, con indirizzi IP diversi o con gli stessi indirizzi IP.

>[!NOTE]
>Per impostazione predefinita, un ambito di una zona esistente nelle zone DNS. Questo ambito di zona ha lo stesso nome della zona e le operazioni DNS legacy funzionano in questo ambito.

Lo scenario precedente sul bilanciamento del carico dell'applicazione illustra come configurare tre ambiti di zona per i Data Center in America del Nord.

Con i comandi riportati di seguito, è possibile creare altri due ambiti di zona, uno per i Data Center Dublin e Amsterdam. 

È possibile aggiungere questi ambiti di zona senza apportare modifiche ai tre ambiti di America del Nord zone esistenti nella stessa zona. Inoltre, dopo aver creato questi ambiti di zona, non è necessario riavviare il server DNS.

È possibile utilizzare i seguenti comandi di Windows PowerShell per creare ambiti di zona.

    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
    

Per ulteriori informazioni, vedere [Add-DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

### <a name="bkmk_records2"></a>Aggiungere record agli ambiti di zona

A questo punto è necessario aggiungere i record che rappresentano l'host del server Web negli ambiti di zona.

I record per i Data Center America sono stati aggiunti nello scenario precedente. È possibile usare i comandi di Windows PowerShell seguenti per aggiungere record agli ambiti di zona per i data center europei.
 
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
    

Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

### <a name="bkmk_policies2"></a>Creazione dei criteri DNS

Dopo aver creato le partizioni (ambiti di zona) ed è stato aggiunto un record, è necessario creare criteri DNS per la distribuzione delle query in ingresso in questi ambiti.

Per questo esempio, la distribuzione delle query tra server applicazioni in data center diversi soddisfa i criteri seguenti.

1. Quando la query DNS viene ricevuta da un'origine in una subnet del client nordamericana, il 50% delle risposte DNS puntano al data center di Seattle, il 25% delle risposte puntano al datacenter di Chicago e il 25% delle risposte rimanenti puntano al Data Center Dallas.
2. Quando la query DNS viene ricevuta da un'origine in una subnet client europea, il 50% delle risposte DNS punta al datacenter di Dublino e il 50% delle risposte DNS punta al Data Center di Amsterdam.
3. Quando la query viene eseguita da qualsiasi altra parte del mondo, le risposte DNS vengono distribuite tra tutti e cinque i Data Center.

Per implementare questi criteri DNS, è possibile usare i comandi di Windows PowerShell seguenti.

    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
    
    Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
    
    Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
    
    

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

A questo punto è stato creato un criterio DNS che fornisce il bilanciamento del carico dell'applicazione tra server Web che si trovano in cinque diversi Data Center in più continenti.

È possibile creare migliaia di criteri DNS in base del traffico di gestione e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, per le query in ingresso.
