---
title: Usare i criteri DNS per l'applicazione del bilanciamento del carico
description: Questo argomento fa parte del DNS criteri Scenario Guide per Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dca60fc0e216b1b873bd4f94dd1b01174d80fc14
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446446"
---
# <a name="use-dns-policy-for-application-load-balancing"></a>Usare i criteri DNS per l'applicazione del bilanciamento del carico

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come configurare criteri DNS per eseguire il bilanciamento del carico dell'applicazione.

Le versioni precedenti di Windows Server DNS fornivano solo il bilanciamento del carico con le risposte di round robin ma con il servizio DNS in Windows Server 2016, è possibile configurare criteri DNS per il bilanciamento del carico dell'applicazione.

Quando si distribuiscono più istanze di un'applicazione, è possibile utilizzare criteri DNS per bilanciare il carico del traffico tra le istanze dell'applicazione diversi, in tal modo dinamicamente allocando il carico del traffico per l'applicazione.

## <a name="example-of-application-load-balancing"></a>Esempio di bilanciamento del carico dell'applicazione

Ecco un esempio di come è possibile utilizzare criteri DNS per il bilanciamento del carico dell'applicazione.

In questo esempio viene utilizzata una società fittizia - servizi regalo Contoso - che fornisce servizi gifing online e che dispone di un sito Web denominato **contosogiftservices.com**.

Il sito Web contosogiftservices.com è ospitato in più Data Center che dispongono di indirizzi IP diversi.

In America del Nord, ovvero il mercato primario per servizi regalo Contoso, il sito Web è ospitato in tre Data Center: Chicago, IL, Dallas, Texas e Seattle, WA.

Il server Web di Seattle è la migliore configurazione hardware e può gestire due volte la quantità di carico come gli altri due siti. Servizi regalo Contoso desidera che il traffico delle applicazioni indirizzato nel modo seguente.

- Poiché il server Web di Seattle include più risorse, la metà dei client dell'applicazione vengono indirizzata a questo server
- Un quarto del client dell'applicazione vengono indirizzati al Data Center di Dallas, Texas
- Un quarto del client dell'applicazione vengono indirizzate a Chicago, IL Data Center

Nella figura seguente viene illustrato questo scenario.

![DNS applicazione bilanciamento del carico con i criteri DNS](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>Applicazione come il bilanciamento del carico Works

Dopo aver configurato il server DNS con i criteri DNS per applicazione carico bilanciamento del carico con questo scenario di esempio, il server DNS risponde 50% del tempo con l'indirizzo del server Web di Seattle, 25% del tempo con l'indirizzo del server Web di Dallas e il 25% del tempo con l'indirizzo del server Web a Chicago.

In questo modo per tutte le quattro query che il server DNS riceve, risponde con due risposte per Seattle e una per Chicago e Dallas.

Un possibile problema con il bilanciamento del carico con i criteri di DNS è la memorizzazione nella cache di record DNS per il client DNS e il resolver/LDNS, che può interferire con il bilanciamento del carico perché il client o il sistema di risoluzione non inviare una query al server DNS.

È possibile ridurre l'effetto di questo comportamento usando un oraria bassa\-al\-Live \(durata (TTL)\) valore per i record DNS che deve essere con carico bilanciato.

### <a name="how-to-configure-application-load-balancing"></a>Come configurare Bilanciamento del carico dell'applicazione

Le sezioni seguenti illustrano come configurare criteri DNS per il bilanciamento del carico dell'applicazione.

#### <a name="create-the-zone-scopes"></a>Creare gli ambiti di zona

È innanzitutto necessario creare gli ambiti di contosogiftservices.com la zona per i Data Center in cui sono ospitati.

Un ambito di una zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona che contiene un proprio set di record DNS. Lo stesso record possono essere presenti in più ambiti, con diversi indirizzi IP o gli stessi indirizzi IP.

>[!NOTE]
>Per impostazione predefinita, un ambito di una zona esistente nelle zone DNS. In questo ambito di zona ha lo stesso nome della zona e operazioni DNS legacy funzionano in questo ambito.

È possibile utilizzare i seguenti comandi di Windows PowerShell per creare ambiti di zona.
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"

Per altre informazioni, vedere [Aggiungi DnsServerZoneScope](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverzonescope?view=win10-ps)

#### <a name="bkmk_records"></a>Aggiungere i record per gli ambiti di zona

A questo punto è necessario aggiungere i record che rappresenta l'host del server web negli ambiti di zona.

Nelle **SeattleZoneScope**, è possibile aggiungere il record www.contosogiftservices.com con indirizzo IP 192.0.0.1, che si trova nel datacenter di Seattle.

Nelle **ChicagoZoneScope**, è possibile aggiungere lo stesso record \(www.contosogiftservices.com\) con indirizzo IP 182.0.0.1 nel Data Center a Chicago.

In modo analogo in **DallasZoneScope**, è possibile aggiungere un record \(www.contosogiftservices.com\) con indirizzo IP 162.0.0.1 nel Data Center a Chicago.

È possibile utilizzare i seguenti comandi di Windows PowerShell per aggiungere record per gli ambiti di zona.
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
    

Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverresourcerecord?view=win10-ps).

#### <a name="bkmk_policies"></a>Creare i criteri DNS

Dopo aver creato le partizioni (ambiti zona) e aver aggiunto i record, è necessario creare criteri DNS che distribuiscono le query in ingresso tra questi ambiti, in modo da 50% delle query per contosogiftservices.com riceve una risposta con l'indirizzo IP per il Web Server nel datacenter di Seattle e gli altri vengono distribuite equamente tra i Data Center a Chicago e Dallas.

È possibile usare i comandi di Windows PowerShell seguenti per creare un criterio DNS che bilancia il traffico dell'applicazione tra questi tre Data Center.

>[!NOTE]
>Nell'esempio di comando seguente, l'espressione – ZoneScope "SeattleZoneScope, 2. ChicagoZoneScope, 1; DallasZoneScope, 1" Configura il server DNS con una matrice che include la combinazione di parametri \<ZoneScope\>,\<peso\>.
    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
    

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://docs.microsoft.com/powershell/module/dnsserver/add-dnsserverqueryresolutionpolicy?view=win10-ps).  

È stato correttamente creato un criterio DNS che fornisce applicazioni bilanciamento del carico tra i server Web in tre Data Center diversi.

È possibile creare migliaia di criteri DNS in base del traffico di gestione e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, per le query in ingresso.
