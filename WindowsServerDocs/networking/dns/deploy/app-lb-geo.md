---
title: Usare i criteri DNS per l'applicazione del bilanciamento del carico con la consapevolezza della posizione geografica
description: Questo argomento fa parte del DNS criteri Scenario Guide per Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: b6e679c6-4398-496c-88bc-115099f3a819
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 806c0cdeedb44db44fc0ec5218124f516a6f70e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852552"
---
# <a name="use-dns-policy-for-application-load-balancing-with-geo-location-awareness"></a>Usare i criteri DNS per l'applicazione del bilanciamento del carico con la consapevolezza della posizione geografica

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come configurare criteri DNS per un'applicazione di bilanciare il carico con la consapevolezza della posizione geografica.

In questa Guida, argomento precedente [usare i criteri DNS per il bilanciamento del carico dell'applicazione](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb), viene utilizzato un esempio di una società fittizia - servizi regalo Contoso - che fornisce alle relative alle donazioni online services e che dispone di un sito Web denominato contosogiftservices.com. Bilancia il carico di servizi regalo Contoso propria applicazione Web online tra i server in Nord americano Data Center dislocati in Seattle, WA, Chicago, IL e Dallas, Texas.

>[!NOTE]
>È consigliabile acquisire familiarità con l'argomento [usare i criteri DNS per il bilanciamento del carico dell'applicazione](https://technet.microsoft.com/windows-server-docs/networking/dns/deploy/app-lb) prima di eseguire le istruzioni riportate in questo scenario.

In questo argomento Usa la stessa società fittizia e infrastruttura di rete come base per una nuova distribuzione di esempio che include la consapevolezza della posizione geografica.

In questo esempio, servizi regalo Contoso sta espandendo correttamente la propria presenza in tutto il mondo.

Analogamente a America del Nord, la società dispone ora i server web ospitati in data center europeo.

Gli amministratori DNS di servizi regalo Contoso desidera configurare applicazione bilanciamento del carico per i data center europeo in modo simile all'implementazione di criteri DNS in Stati Uniti, con il traffico delle applicazioni distribuito tra i server Web che si trovano in Dublino, Amsterdam, Olanda e altrove.

Gli amministratori DNS vuole inoltre che tutte le query da altre posizioni in tutto il mondo distribuita equamente tra tutti i propri centri dati.

Nelle sezioni successive è possibile imparare a raggiungere gli obiettivi simili a quelle degli amministratori DNS Contoso nella propria rete.

## <a name="how-to-configure-application-load-balancing-with-geo-location-awareness"></a>Come configurare l'applicazione bilanciamento del carico con la consapevolezza della posizione geografica

Le sezioni seguenti illustrano come configurare criteri DNS per applicazione bilanciamento del carico con la consapevolezza della posizione geografica.

>[!IMPORTANT]
>Nelle sezioni seguenti includono esempi di comandi Windows PowerShell che contengono valori di esempio per numero di parametri. Assicurarsi di sostituire i valori di esempio in questi comandi con i valori appropriati per la distribuzione prima di eseguire questi comandi.

###<a name="bkmk_clientsubnets"></a>Creare la subnet del Client DNS

È necessario innanzitutto identificare la subnet o spazio di indirizzi IP delle aree di America del Nord ed Europa.

È possibile ottenere queste informazioni da mappe Geo-IP. Basato su queste distribuzioni geografica IP, è necessario creare la subnet del Client DNS.

Una Subnet del Client DNS è un raggruppamento logico di subnet IPv4 o IPv6 da cui le query vengono inviate a un server DNS.

È possibile utilizzare i seguenti comandi di Windows PowerShell per creare subnet del Client DNS. 

    
    Add-DnsServerClientSubnet -Name "AmericaSubnet" -IPv4Subnet 192.0.0.0/24,182.0.0.0/24
    Add-DnsServerClientSubnet -Name "EuropeSubnet" -IPv4Subnet 141.1.0.0/24,151.1.0.0/24
    
Per ulteriori informazioni, vedere [Aggiungi DnsServerClientSubnet](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverclientsubnet?view=win10-ps).

###<a name="bkmk_zscopes2"></a>Creare gli ambiti di zona

Dopo che la subnet del client è presenti, è necessario partizionare la zona contosogiftservices.com in ambiti diversi zona, ognuno per un Data Center.

Un ambito di una zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona che contiene un proprio set di record DNS. Lo stesso record possono essere presenti in più ambiti, con diversi indirizzi IP o gli stessi indirizzi IP.

>[!NOTE]
>Per impostazione predefinita, un ambito di una zona esistente nelle zone DNS. In questo ambito di zona ha lo stesso nome della zona e operazioni DNS legacy funzionano in questo ambito.

Lo scenario precedente sul bilanciamento del carico dell'applicazione viene illustrato come configurare tre ambiti di zona per i Data Center in America del Nord.

Con i comandi seguenti, è possibile creare due ambiti di zona altre, rispettivamente per i Data Center Dublino e Amsterdam. 

È possibile aggiungere questi ambiti di zona senza apportare modifiche a tre ambiti di zona America del Nord esistenti nella stessa zona. Inoltre, dopo aver creato questi ambiti di zona, è necessario riavviare il server DNS.

È possibile utilizzare i seguenti comandi di Windows PowerShell per creare ambiti di zona.

    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DublinZoneScope"
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "AmsterdamZoneScope"
    

Per altre informazioni, vedere [Aggiungi DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

###<a name="bkmk_records2"></a>Aggiungere i record per gli ambiti di zona

A questo punto è necessario aggiungere i record che rappresenta l'host del server web negli ambiti di zona.

Nello scenario precedente sono stati aggiunti i record per i Data Center America. È possibile usare i comandi di Windows PowerShell seguenti per aggiungere record per gli ambiti di zona per i data center europeo.
 
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "151.1.0.1" -ZoneScope "DublinZoneScope”
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "141.1.0.1" -ZoneScope "AmsterdamZoneScope"
    

Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

###<a name="bkmk_policies2"></a>Creare i criteri DNS

Dopo aver creato le partizioni (ambiti zona) e aver aggiunto i record, è necessario creare criteri DNS che distribuiscono le query in ingresso tra questi ambiti.

Per questo esempio, la distribuzione di query tra i server applicazioni in diversi Data Center soddisfa i criteri seguenti.

1. Quando la query DNS viene ricevuta da un'origine in una subnet del client Nord America, 50% delle risposte DNS punti al data center di Seattle, 25% delle risposte punti al Data Center a Chicago e puntare il restante 25% delle risposte nel Data Center di Dallas.
2. Quando la query DNS viene ricevuta da un'origine in una subnet del client europei, 50% delle risposte DNS punti al Data Center Dublino e 50% delle risposte DNS punti al Data Center Amsterdam.
3. Quando la query proviene dal altrove nel mondo, le risposte DNS vengono distribuite tra tutti i data cinque Center.

È possibile utilizzare i comandi di Windows PowerShell seguenti per implementare questi criteri DNS.

    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaLBPolicy" -Action ALLOW -ClientSubnet "eq,AmericaSubnet" -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1; TexasZoneScope,1" -ZoneName "contosogiftservices.com" –ProcessingOrder 1
    
    Add-DnsServerQueryResolutionPolicy -Name "EuropeLBPolicy" -Action ALLOW -ClientSubnet "eq,EuropeSubnet" -ZoneScope "DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 2
    
    Add-DnsServerQueryResolutionPolicy -Name "WorldWidePolicy" -Action ALLOW -FQDN "eq,*.contoso.com" -ZoneScope "SeattleZoneScope,1;ChicagoZoneScope,1; TexasZoneScope,1;DublinZoneScope,1;AmsterdamZoneScope,1" -ZoneName "contosogiftservices.com" -ProcessingOrder 3
    
    

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).

È stato correttamente creato un criterio DNS che fornisce applicazioni bilanciamento del carico tra server Web che si trovano in cinque diversi Data Center in più continenti.

È possibile creare migliaia di criteri DNS in base del traffico di gestione e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, per le query in ingresso.
