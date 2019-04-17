---
title: Usare i criteri DNS per l'applicazione bilanciamento del carico con la consapevolezza della posizione geografica
description: In questo argomento fa parte di DNS criteri Scenario Guide per Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7637d927c7b22b83053e7f9100b07581c11bafc0
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>Usare i criteri DNS per l'applicazione bilanciamento del carico con la consapevolezza della posizione geografica

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come configurare criteri DNS per bilanciare un'applicazione con la consapevolezza della posizione geografica.

L'argomento precedente in questa Guida, [usare i criteri DNS per il bilanciamento del carico applicazione](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb), Usa un esempio di una società fittizia - servizi regalo Contoso - che fornisce regalare online services e che dispone di un sito Web denominato contosogiftservices.com. Bilancia il carico servizi regalo Contoso delle applicazioni Web tra server in Nord America centri dati che si trovano in Seattle, Washington, Chicago, IL e Roma, Italia online.

>[!NOTE]
>È consigliabile acquisire familiarità con l'argomento [usare i criteri DNS per il bilanciamento del carico applicazione](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb) prima di eseguire le istruzioni in questo scenario.

In questo argomento utilizza la stessa società fittizia e l'infrastruttura di rete come base per una nuova distribuzione di esempio che include la consapevolezza della posizione geografica.

In questo esempio, servizi regalo Contoso correttamente in espansione la loro presenza in tutto il mondo.

Simile a America del Nord, la società dispone ora il server web ospitato nel data center dell'Europa.

Gli amministratori DNS di servizi regalo Contoso desidera configurare applicazione bilanciamento del carico per Data Center dell'Europa in modo simile all'implementazione di criteri DNS negli Stati Uniti, con traffico dell'applicazione distribuito tra i server Web che si trovano in Olanda Dublino, Irlanda, Amsterdam e un' posizione.

Gli amministratori DNS desiderare di tutte le query in altre posizioni nel mondo distribuite equamente tra tutti i propri centri dati.

Nelle sezioni successive possono imparare raggiungere obiettivi simili a quelle degli amministratori Contoso DNS sulla rete.

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>Come configurare l'applicazione bilanciamento del carico con la consapevolezza della posizione geografica

Le sezioni seguenti illustrano come configurare criteri DNS per l'applicazione bilanciamento del carico con la consapevolezza della posizione geografica.

>[!IMPORTANT]
>Le sezioni seguenti includono esempi di comandi Windows PowerShell che contengono i valori di esempio per numero di parametri. Assicurarsi di sostituire i valori di esempio in questi comandi con i valori appropriati per la distribuzione prima di eseguire questi comandi.

###<a name="bkmk_clientsubnets"></a>Creare la subnet del Client DNS

È innanzitutto necessario identificare la subnet o spazio di indirizzi IP delle aree Europa e America del Nord.

È possibile ottenere queste informazioni da mappe Geo-IP. In base a queste distribuzioni geografica IP, è necessario creare la subnet del Client DNS.

Una Subnet del Client DNS è un raggruppamento logico di subnet IPv4 o IPv6 da cui le query vengono inviate a un server DNS.

È possibile utilizzare i seguenti comandi di Windows PowerShell per creare subnet del Client DNS. 

    
    Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
    
Per ulteriori informazioni, vedere [Aggiungi DnsServerClientSubnet](https://technet.microsoft.com/library/mt126261.aspx).

###<a name="bkmk_zscopes2"></a>Creare gli ambiti di zona

Dopo la subnet del client, è necessario partizionare la zona contosogiftservices.com in ambiti diversi zona, ciascuno di essi per un Data Center.

Un ambito di zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona che contiene un proprio set di record DNS. Lo stesso record possono essere presenti in più ambiti, con indirizzi IP diversi o gli stessi indirizzi IP.

>[!NOTE]
>Per impostazione predefinita, un ambito di una zona esistente nelle zone DNS. Questo ambito di zona con lo stesso nome della zona e operazioni DNS legacy funzionano in questo ambito.

Lo scenario precedente sul bilanciamento del carico di applicazioni viene illustrato come configurare tre ambiti di zona per centri dati in Nord America.

Con i comandi seguenti, è possibile creare due ambiti di zona più, uno per il Data Center Dublin e Amsterdam. 

È possibile aggiungere questi ambiti di zona senza apportare modifiche a tre ambiti di zona America del Nord esistenti nella stessa area. Inoltre, dopo aver creato questi ambiti di zona, non è necessario riavviare il server DNS.

È possibile utilizzare i seguenti comandi di Windows PowerShell per creare ambiti di zona.

    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
    

Per ulteriori informazioni, vedere [Aggiungi DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

###<a name="bkmk_records2"></a>Aggiungere record per gli ambiti di zona

Ora è necessario aggiungere i record che rappresenta l'host del server web negli ambiti di zona.

Sono stati aggiunti i record per il Data Center America nello scenario precedente. È possibile utilizzare i seguenti comandi di Windows PowerShell per aggiungere record per gli ambiti di zona per Data Center dell'Europa.
 
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
    

Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).

###<a name="bkmk_policies2"></a>Creare i criteri DNS

Dopo aver creato le partizioni (ambiti zona) ed è stato aggiunto record, è necessario creare criteri DNS che distribuiscono le query in ingresso tra questi ambiti.

Per questo esempio, la distribuzione di query in server delle applicazioni in diversi Data Center soddisfi i criteri seguenti.

1. Quando la query DNS viene ricevuta da un'origine in una subnet del client America del Nord, 50% delle risposte DNS scegliere il data center di Seattle, 25% delle risposte puntare al Data Center Chicago e scegliere il 25% rimanenti di risposte al Data Center Roma.
2. Quando la query DNS viene ricevuta da un'origine in una subnet del client europei, 50% delle risposte DNS scegliere Data Center Dublin e scegliere il Data Center Amsterdam 50% delle risposte DNS.
3. Quando la query proviene da altrove nel mondo, le risposte DNS vengono distribuite su tutti e cinque centri dati.

È possibile utilizzare i seguenti comandi di Windows PowerShell per implementare i criteri DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
    
    Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
    
    Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
    
    

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).

Hai ora creato correttamente un criterio DNS che offre applicazioni bilanciamento del carico tra i server Web che si trovano in cinque diversi Data Center in più continenti.

È possibile creare migliaia di criteri DNS in base del traffico di gestione, e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, su query in ingresso.
