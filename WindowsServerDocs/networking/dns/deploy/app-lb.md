---
title: Usare i criteri DNS per Bilanciamento carico di applicazioni
description: In questo argomento fa parte di DNS criteri Scenario Guide per Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: f9c313ac-bb86-4e48-b9b9-de5004393e06
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d156c258b971c45bf1c4c20739440bd5cc9e239f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="use-dns-policy-for-application-load-balancing"></a>Usare i criteri DNS per Bilanciamento carico di applicazioni

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come configurare criteri DNS per eseguire il bilanciamento del carico di applicazioni.

Le versioni precedenti di Windows Server DNS fornito solo bilanciamento del carico usando le risposte round robin. ma con DNS in Windows Server 2016, è possibile configurare criteri DNS per il bilanciamento del carico di applicazioni.

Quando si distribuiscono più istanze di un'applicazione, è possibile utilizzare criteri DNS per bilanciare il carico del traffico tra le istanze di applicazioni diverso, allocazione dinamica in tal modo il carico del traffico per l'applicazione.

## <a name="example-of-application-load-balancing"></a>Esempio di bilanciamento del carico di applicazioni

Ecco un esempio di come è possibile utilizzare criteri DNS per il bilanciamento del carico dell'applicazione.

Questo esempio Usa una società fittizia - servizi regalo Contoso - che fornisce servizi gifing online e che dispone di un sito Web denominato **contosogiftservices.com**.

Il sito Web contosogiftservices.com è ospitato in più centri dati che dispongono di indirizzi IP diversi.

In America del Nord, ovvero il mercato primario per servizi regalo Contoso, il sito Web è ospitato in centri tre dati: Chicago, IL, Roma, Italia e Seattle, Washington.

Il server Web di Seattle è la migliore configurazione hardware e gestione doppio del carico come i due siti. Servizi regalo Contoso desidera il traffico delle applicazioni indicato nel modo seguente.

- Poiché il server Web di Seattle include altre risorse, metà del client dell'applicazione vengono indirizzate a questo server
- Un quarto dei client dell'applicazione vengono indirizzate al Data Center Roma, Italia
- Un quarto dei client dell'applicazione vengono indirizzati a Chicago, IL Data Center

Nella figura seguente viene illustrato questo scenario.

![DNS applicazione bilanciamento del carico con i criteri DNS](../../media/Dns-App-Lb/dns-app-lb.jpg)


### <a name="how-application-load-balancing-works"></a>Applicazione come bilanciamento del carico Works

Dopo aver configurato il server DNS con i criteri DNS per l'applicazione carico bilanciamento del carico usando questo scenario di esempio, il server DNS risponde 50% del tempo con l'indirizzo del server Web di Seattle, 25% del tempo con l'indirizzo del server Web di Roma e 25% del tempo con l'indirizzo del server Web a Chicago.

Di conseguenza per ogni quattro query che il server DNS riceve, risponde con due risposte per Seattle e uno per Roma e Chicago.

Un possibile problema con il bilanciamento del carico con i criteri DNS è la memorizzazione nella cache di record DNS per il client DNS e il resolver/LDNS, che può interferire con il bilanciamento del carico in quanto i resolver client invia una query al server DNS.

Consente di ridurre l'effetto di questo comportamento usando un valore basso \(TTL\) trascorso-da destra a in tempo reale per i record DNS che devono essere con carico bilanciato.

### <a name="how-to-configure-application-load-balancing"></a>Come configurare Bilanciamento carico di applicazioni

Le sezioni seguenti illustrano come configurare criteri DNS per il bilanciamento del carico di applicazioni.

#### <a name="create-the-zone-scopes"></a>Creare gli ambiti di zona

È innanzitutto necessario creare gli ambiti di contosogiftservices.com di zona per i centri dati in cui sono ospitati.

Un ambito di zona è un'istanza univoca della zona. Una zona DNS può avere più ambiti di zona, con ogni ambito di zona che contiene un proprio set di record DNS. Lo stesso record possono essere presenti in più ambiti, con indirizzi IP diversi o gli stessi indirizzi IP.

>[!NOTE]
>Per impostazione predefinita, un ambito di una zona esistente nelle zone DNS. Questo ambito di zona con lo stesso nome della zona e operazioni DNS legacy funzionano in questo ambito.

È possibile utilizzare i seguenti comandi di Windows PowerShell per creare ambiti di zona.
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "SeattleZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "DallasZoneScope"
    
    Add-DnsServerZoneScope -ZoneName "contosogiftservices.com" -Name "ChicagoZoneScope"

Per ulteriori informazioni, vedere [Aggiungi DnsServerZoneScope](https://technet.microsoft.com/library/mt126267.aspx)

####<a name="bkmk_records"></a>Aggiungere record per gli ambiti di zona

Ora è necessario aggiungere i record che rappresenta l'host del server web negli ambiti di zona.

In **SeattleZoneScope**, è possibile aggiungere il record www.contosogiftservices.com con indirizzo IP 192.0.0.1, che si trova nel datacenter di Seattle.

In **ChicagoZoneScope**, è possibile aggiungere la stessa \(www.contosogiftservices.com\) record con indirizzo IP 182.0.0.1 nel Data Center Chicago.

Allo stesso modo in **DallasZoneScope**, è possibile aggiungere un record \(www.contosogiftservices.com\) con indirizzo IP 162.0.0.1 nel Data Center Chicago.

È possibile utilizzare i seguenti comandi di Windows PowerShell per aggiungere record per gli ambiti di zona.
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "192.0.0.1" -ZoneScope "SeattleZoneScope
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "182.0.0.1" -ZoneScope "ChicagoZoneScope"
    
    Add-DnsServerResourceRecord -ZoneName "contosogiftservices.com" -A -Name "www" -IPv4Address "162.0.0.1" -ZoneScope "DallasZoneScope"
    

Per ulteriori informazioni, vedere [Aggiungi DnsServerResourceRecord](https://technet.microsoft.com/library/jj649925.aspx).

####<a name="bkmk_policies"></a>Creare i criteri DNS

Dopo aver creato le partizioni (ambiti zona) ed è stato aggiunto record, è necessario creare criteri DNS che distribuiscono le query in ingresso tra questi ambiti in modo da 50% delle query per contosogiftservices.com ha risposto a con l'indirizzo IP del server Web nel datacenter di Seattle e il resto vengono distribuite equamente tra il Data Center a Chicago e Roma.

È possibile utilizzare i seguenti comandi di Windows PowerShell per creare un criterio DNS per bilanciare il traffico delle applicazioni in questi tre datacenter.

>[!NOTE]
>Nell'esempio di comando sotto, l'espressione – ZoneScope "SeattleZoneScope, 2. ChicagoZoneScope, 1. DallasZoneScope, 1" consente di configurare il server DNS con una matrice che include la combinazione di parametro \ < ZoneScope\, > \ < weight\ >.
    
    Add-DnsServerQueryResolutionPolicy -Name "AmericaPolicy" -Action ALLOW – -ZoneScope "SeattleZoneScope,2;ChicagoZoneScope,1;DallasZoneScope,1" -ZoneName "contosogiftservices.com"
    

Per ulteriori informazioni, vedere [Aggiungi DnsServerQueryResolutionPolicy](https://technet.microsoft.com/library/mt126273.aspx).  

Hai ora creato correttamente un criterio DNS che offre applicazioni bilanciamento del carico tra i server Web in tre diversi Data Center.

È possibile creare migliaia di criteri DNS in base del traffico di gestione, e tutti i nuovi criteri vengono applicati in modo dinamico, senza il riavvio del server DNS, su query in ingresso.